2025030621:57
___
Date: 06-03-2025 | 21:57
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Команды Redis

Да, я использовал **Redis** в нескольких проектах, в основном для кэширования, управления очередями и хранения временных данных. Работал с ним через Go с библиотекой `go-redis`, а также напрямую через CLI для тестирования. Вот мой опыт и команды, которые я знаю и применял:

### Опыт использования

- **Кэширование**: В одном проекте кэшировал результаты запросов к базе данных (например, профили пользователей) в Redis, чтобы снизить нагрузку на PostgreSQL. Использовал TTL для автоматического обновления.
- **Очереди**: Настраивал простые очереди задач для асинхронной обработки (например, отправка email).
- **Счетчики**: Применял Redis для подсчета метрик (например, просмотры страниц) с атомарными операциями.

### Команды Redis, которые знаю

Redis поддерживает множество команд для работы с разными типами данных. Вот основные, с которыми я работал:

#### Строки (Strings)

- **SET key value [EX seconds]**: Устанавливает значение по ключу с опциональным TTL.
    - Пример: SET user:1 "Alice" EX 60 — ключ живет 60 секунд.
- **GET key**: Получает значение по ключу.
    - Пример: GET user:1 — вернет "Alice".
- **INCR key**: Атомарно увеличивает значение (для счетчиков).
    - Пример: INCR views — увеличивает счетчик просмотров.
- **DEL key**: Удаляет ключ.
    - Пример: DEL user:1 — удаляет ключ.

#### Списки (Lists)

- **LPUSH key value**: Добавляет значение в начало списка (левая сторона).
    - Пример: LPUSH tasks "send-email" — добавляет задачу в очередь.
- **RPOP key**: Извлекает и удаляет значение с конца списка (правая сторона).
    - Пример: RPOP tasks — берет задачу из очереди.
- **LLEN key**: Возвращает длину списка.
    - Пример: LLEN tasks — сколько задач в очереди.

#### Хэши (Hashes)

- **HSET key field value**: Устанавливает поле в хэше.
    - Пример: HSET user:1 name "Alice" — задает имя в хэше.
- **HGET key field**: Получает значение поля.
    - Пример: HGET user:1 name — вернет "Alice".
- **HGETALL key**: Получает все поля и значения хэша.
    - Пример: HGETALL user:1 — полный хэш.

#### Множества (Sets)

- **SADD key member**: Добавляет элемент в множество.
    - Пример: SADD friends:1 "Bob" — добавляет друга.
- **SMEMBERS key**: Возвращает все элементы множества.
    - Пример: SMEMBERS friends:1 — список друзей.

#### Отсортированные множества (Sorted Sets)

- **ZADD key score member**: Добавляет элемент с весом.
    - Пример: ZADD leaderboard 100 "Alice" — добавляет игрока в рейтинг.
- **ZRANGE key start stop**: Возвращает элементы в диапазоне.
    - Пример: ZRANGE leaderboard 0 2 — топ-3 игрока.

#### Пример кода с комментариями
##### Пример использования команд в Go с go-redis:
```go
package main

import (
    "context"
    "fmt"
    "time"

    "github.com/go-redis/redis/v8"
)

func main() {
    client := redis.NewClient(&redis.Options{
        Addr: "localhost:6379", // подключение к Redis
    })
    ctx := context.Background()

    // SET с TTL
    err := client.Set(ctx, "cache:key", "data", 10*time.Second).Err()
    if err != nil {
        fmt.Println("SET ошибка:", err)
    }

    // GET
    val, err := client.Get(ctx, "cache:key").Result()
    if err != nil {
        fmt.Println("GET ошибка:", err)
    } else {
        fmt.Println("GET:", val) // GET: data
    }

    // LPUSH и RPOP для очереди
    client.LPush(ctx, "queue", "task1")
    task, err := client.RPop(ctx, "queue").Result()
    if err != nil {
        fmt.Println("RPOP ошибка:", err)
    } else {
        fmt.Println("Task:", task) // Task: task1
    }

    // INCR для счетчика
    count, err := client.Incr(ctx, "counter").Result()
    if err != nil {
        fmt.Println("INCR ошибка:", err)
    } else {
        fmt.Println("Counter:", count) // Counter: 1
    }
}
```

- **Комментарий**:
    - Set с TTL кэширует данные на 10 секунд.
    - LPush/RPop эмулируют очередь.
    - Incr увеличивает счетчик атомарно.

### Итог

Я использовал Redis для кэширования, очередей и метрик, применяя команды вроде SET, GET, INCR, LPUSH, HSET, ZADD. Это позволило ускорить приложения и организовать асинхронные процессы. Знаю базовые команды и могу расширить функционал при необходимости.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
