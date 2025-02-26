2025022501:28
___
Date: 25-02-2025 | 01:28
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Запись и чтение в CQRS

Разделение "записи" и "чтения" в CQRS действительно подразумевает, что данные из хранилища для записи (write store, например, PostgreSQL) должны каким-то образом попасть в хранилище для чтения (read store, например, Redis, Elasticsearch или даже другая таблица в той же базе). Это требует механизма синхронизации между ними, и в реальных системах это обычно делается асинхронно.

---
### Почему данные не появляются автоматически?

В CQRS хранилища для записи и чтения независимы. Write store оптимизировано для операций изменения (нормализованная структура, транзакции, ACID), а read store — для быстрого чтения (денормализованные данные, кэши). После записи в PostgreSQL данные не "перетекают" сами в другое хранилище — нужен процесс, который это обеспечит.

---
### Как данные попадают из write store в read store?

Синхронизация чаще всего реализуется через **события** (event-driven подход). Вот общий процесс:
  
1. **Команда изменяет состояние**: Выполняется команда (например, **CreateUser**), данные записываются в write store.
2. **Генерируется событие**: После успешной записи создается событие (например, **UserCreated**), описывающее изменение.
3. **Событие передается**: Оно отправляется в промежуточный слой (например, очередь сообщений).
4. **Обработчик обновляет read store**: Событие обрабатывается, и данные записываются в read store в нужном формате.
  
---
### Основные подходы к реализации

#### 1. Синхронное обновление (простейший вариант)

После записи в PostgreSQL вы сразу обновляете read store в той же транзакции. Это редко используется из-за потери преимуществ CQRS, но подходит для простых случаев
#### 2. Асинхронное обновление через очередь сообщений

Самый распространенный подход:
- Используется брокер сообщений (Kafka, RabbitMQ, NATS).
- Write store публикует событие в очередь.
- Read store подписывается на события и обновляется.
#### 3. Event Sourcing

Вместо хранения текущего состояния в write store хранится только журнал событий. Read store строится как проекция этих событий.

Мы разберем вариант с асинхронным обновлением через очередь, так как это стандарт для CQRS.

---
### Пример реализации в Go

Допустим, у нас есть:
- **Write store**: PostgreSQL для записи пользователей.
- **Read store**: Redis для быстрого чтения.
- **Очередь**: NATS для передачи событий.

#### Шаг 1: Запись в PostgreSQL и генерация события
```go
package main

import (
    "context"
    "database/sql"
    "encoding/json"
    "fmt"
    _ "github.com/lib/pq"
    "github.com/nats-io/nats.go"
)

// User — структура для записи
type User struct {
    ID    int
    Name  string
    Email string
}

// UserCreatedEvent — событие
type UserCreatedEvent struct {
    ID    int
    Name  string
    Email string
}

func createUser(db *sql.DB, nc *nats.Conn, name, email string) error {
    tx, err := db.BeginTx(context.Background(), nil)
    if err != nil {
        return err
    }
    defer tx.Rollback()

    // Записываем в PostgreSQL
    var id int
    err = tx.QueryRow("INSERT INTO users (name, email) VALUES ($1, $2) RETURNING id", name, email).Scan(&id)
    if err != nil {
        return err
    }

    // Генерируем событие
    event := UserCreatedEvent{ID: id, Name: name, Email: email}
    eventData, err := json.Marshal(event)
    if err != nil {
        return err
    }

    // Публикуем событие в NATS
    err = nc.Publish("user.created", eventData)
    if err != nil {
        return err
    }

    return tx.Commit()
}

func main() {
    db, err := sql.Open("postgres", "user=pguser dbname=test sslmode=disable")
    if err != nil {
        fmt.Println("Ошибка подключения к БД:", err)
        return
    }
    defer db.Close()

    nc, err := nats.Connect("nats://localhost:4222")
    if err != nil {
        fmt.Println("Ошибка подключения к NATS:", err)
        return
    }
    defer nc.Close()

    err = createUser(db, nc, "Alice", "alice@example.com")
    if err != nil {
        fmt.Println("Ошибка создания пользователя:", err)
    } else {
        fmt.Println("Пользователь создан")
    }
}
```

**Что происходит?**

1. Пользователь записывается в PostgreSQL.
2. Создается событие **UserCreatedEvent**.
3. Событие публикуется в NATS на тему **user.created**.
  
---
#### Шаг 2: Обновление Redis на основе события
```go
package main

import (
    "context"
    "encoding/json"
    "fmt"
    "github.com/go-redis/redis/v8"
    "github.com/nats-io/nats.go"
)

type UserReadModel struct {
    ID    int    `json:"id"`
    Name  string `json:"name"`
    Email string `json:"email"`
}

func main() {
    // Подключаемся к Redis
    rdb := redis.NewClient(&redis.Options{
        Addr: "localhost:6379",
    })
    defer rdb.Close()

    // Подключаемся к NATS
    nc, err := nats.Connect("nats://localhost:4222")
    if err != nil {
        fmt.Println("Ошибка подключения к NATS:", err)
        return
    }
    defer nc.Close()

    // Подписываемся на события
    _, err = nc.Subscribe("user.created", func(msg *nats.Msg) {
        var event UserCreatedEvent
        if err := json.Unmarshal(msg.Data, &event); err != nil {
            fmt.Println("Ошибка десериализации:", err)
            return
        }

        // Формируем модель для чтения
        readModel := UserReadModel{
            ID:    event.ID,
            Name:  event.Name,
            Email: event.Email,
        }
        data, err := json.Marshal(readModel)
        if err != nil {
            fmt.Println("Ошибка сериализации:", err)
            return
        }

        // Записываем в Redis
        err = rdb.Set(context.Background(), fmt.Sprintf("user:%d", event.ID), data, 0).Err()
        if err != nil {
            fmt.Println("Ошибка записи в Redis:", err)
            return
        }
        fmt.Printf("Пользователь %d добавлен в Redis\n", event.ID)
    })
    if err != nil {
        fmt.Println("Ошибка подписки:", err)
        return
    }

    // Держим приложение запущенным
    select {}
}
```

**Что происходит?**

1. Сервис подписывается на тему **user.created** в NATS.
2. При получении события данные десериализуются.
3. Пользователь сохраняется в Redis в формате, удобном для чтения.
  
---
### Как это работает в продакшене?

1. **Write Store (PostgreSQL)**: Хранит нормализованные данные пользователей.
2. **Событие**: После записи в PostgreSQL публикуется событие в NATS.
3. **Read Store (Redis)**: Отдельный сервис слушает события и обновляет кэш Redis.
4. **Клиент**: Запросы на чтение (**GetUser**) идут напрямую в Redis, минуя PostgreSQL.

**Пример чтения из Redis**
```go
func getUser(rdb *redis.Client, id int) (UserReadModel, error) {
    data, err := rdb.Get(context.Background(), fmt.Sprintf("user:%d", id)).Bytes()
    if err != nil {
        return UserReadModel{}, err
    }
    var user UserReadModel
    if err := json.Unmarshal(data, &user); err != nil {
        return UserReadModel{}, err
    }
    return user, nil
}
```

---
### Нюансы и детали

1. **Eventual Consistency**: Между записью в PostgreSQL и обновлением Redis есть задержка. Это нормально для CQRS — модель чтения становится согласованной со временем.
2. **Обработка сбоев**: Если обновление Redis не удалось, можно:  
    - Повторять попытки (retry).
    - Логировать ошибку и уведомлять систему мониторинга.
3. **Идемпотентность**: Обработчик событий должен быть идемпотентным (повторная обработка того же события не ломает систему).
4. **Сложность**: Нужно управлять очередью, гарантировать доставку событий, обрабатывать дубликаты.
  
---
### Альтернативы

- **Логи базы**: Использовать CDC (Change Data Capture) в PostgreSQL (например, через Debezium) для генерации событий.
- **Синхронное обновление**: Обновлять Redis прямо в **createUser**, но это снижает отказоустойчивость.

---
### Итог

В CQRS данные из PostgreSQL попадают в Redis через события и асинхронную обработку. Это требует брокера сообщений (NATS, Kafka), публикации событий из write store и подписки read store на эти события. В Go это удобно реализовать с использованием горутин и пакетов вроде **nats.go** и **go-redis**.



-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
