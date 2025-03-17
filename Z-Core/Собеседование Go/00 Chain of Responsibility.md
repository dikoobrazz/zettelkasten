2025030712:24
___
Date: 07-03-2025 | 12:24
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Chain of Responsibility

> **Chain of Responsibility** (Цепочка ответственности) — это поведенческий паттерн проектирования, который позволяет передавать запросы по цепочке обработчиков. Каждый обработчик решает, обработать запрос самостоятельно или передать его следующему в цепочке. Это обеспечивает гибкость и разделение ответственности между объектами.

---
### Что такое Chain of Responsibility?

**Определение**: Это структура, где запрос (или задача) передается по цепочке объектов (обработчиков), пока один из них не обработает его или цепочка не закончится.

**Ключевые элементы**:

1. **Handler (Обработчик)**: Интерфейс или абстрактный тип, определяющий метод обработки и ссылку на следующий обработчик.
2. **Concrete Handler (Конкретный обработчик)**: Реализация, которая обрабатывает запрос или передает его дальше.
3. **Client (Клиент)**: Инициирует запрос и запускает цепочку.

**Аналогия**:

- Представьте конвейер в службе поддержки: запрос клиента сначала идет к оператору первого уровня, затем к специалисту, а потом к менеджеру, если никто раньше не решил проблему.

---
### Как работает?

1. Клиент отправляет запрос первому обработчику в цепочке.
2. Обработчик проверяет, может ли он обработать запрос:
    - Если да — обрабатывает и завершает.
    - Если нет — передает следующему обработчику.
3. Процесс продолжается, пока запрос не обработан или цепочка не исчерпана.

---
### Пример в Go

Допустим, мы создаем цепочку для обработки HTTP-запросов с разными уровнями доступа.

- **Интерфейс обработчика**
```go
package main

import "net/http"

// Handler определяет интерфейс для цепочки
type Handler interface {
    SetNext(handler Handler)
    Handle(w http.ResponseWriter, r *http.Request)
}
```

- **Базовая структура обработчика**
```go
type BaseHandler struct {
    next Handler
}

func (h *BaseHandler) SetNext(handler Handler) {
    h.next = handler
}

func (h *BaseHandler) Handle(w http.ResponseWriter, r *http.Request) {
    if h.next != nil {
        h.next.Handle(w, r) // Передаем дальше, если есть следующий
    }
}
```

#### Конкретные обработчики

1. **Проверка аутентификации**:
```go
type AuthHandler struct {
    BaseHandler
}

func (h *AuthHandler) Handle(w http.ResponseWriter, r *http.Request) {
    token := r.Header.Get("Authorization")
    if token == "" {
        http.Error(w, "Unauthorized", http.StatusUnauthorized)
        return
    }
    // Если токен есть, передаем дальше
    if h.next != nil {
        h.next.Handle(w, r)
    }
}
```

2. **Проверка роли**:
```go
type RoleHandler struct {
    BaseHandler
}

func (h *RoleHandler) Handle(w http.ResponseWriter, r *http.Request) {
    role := r.Header.Get("Role") // Упрощение
    if role != "admin" {
        http.Error(w, "Forbidden: admin only", http.StatusForbidden)
        return
    }
    if h.next != nil {
        h.next.Handle(w, r)
    }
}
```

3. **Обработка запроса**:
```go
type APIHandler struct {
    BaseHandler
}

func (h *APIHandler) Handle(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("Request processed successfully"))
}
```

**Сборка цепочки и запуск**
```go
func main() {
    auth := &AuthHandler{}
    role := &RoleHandler{}
    api := &APIHandler{}

    // Создаем цепочку: Auth -> Role -> API
    auth.SetNext(role)
    role.SetNext(api)

    // Тестовый сервер
    http.HandleFunc("/api", func(w http.ResponseWriter, r *http.Request) {
        auth.Handle(w, r)
    })
    http.ListenAndServe(":8080", nil)
}
```

- **Логика**:
    - Если нет токена, `AuthHandler` вернет 401.
    - Если роль не "admin", `RoleHandler` вернет 403.
    - Если всё проходит, `APIHandler` обработает запрос.

**Вывод**:

- `curl http://localhost:8080/api` → "Unauthorized"
- `curl -H "Authorization: token" http://localhost:8080/api` → "Forbidden"
- `curl -H "Authorization: token" -H "Role: admin" http://localhost:8080/api` → "Request processed successfully"

---
### Применение в микросервисах

1. **Middleware в API Gateway**:
    - Цепочка проверок (аутентификация, авторизация, логирование) перед маршрутизацией к сервису.
    - Пример: `Auth -> RateLimit -> Logging -> Service`.
2. **Обработка событий**:
    - В event-driven системах цепочка фильтрует или обрабатывает события (Kafka consumer).
    - Пример: `ValidateEvent -> ProcessEvent -> LogEvent`.
3. **Фильтрация запросов**:
    - Разделение логики обработки в зависимости от типа запроса.

**Пример в микросервисах**:

- `OrderService` проверяет наличие товара, затем передает запрос в `PaymentService`, если проверка прошла.

---
### Плюсы и минусы

**Плюсы**:

- **Гибкость**: Легко добавлять или убирать обработчики.
- **Разделение ответственности**: Каждый обработчик отвечает за свою задачу.
- **Повторное использование**: Обработчики можно комбинировать в разных цепочках.

**Минусы**:

- **Сложность отладки**: Трудно найти, где запрос "застрял".
- **Производительность**: Множественные передачи увеличивают задержку.
- **Риск бесконечной цепочки**: Если неправильно настроить `next`.

---
### Реализация без указателей (альтернатива)

В Go можно использовать функциональный подход:
```go
type Middleware func(http.Handler) http.Handler

func auth(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        if r.Header.Get("Authorization") == "" {
            http.Error(w, "Unauthorized", http.StatusUnauthorized)
            return
        }
        next.ServeHTTP(w, r)
    })
}

func main() {
    handler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Success"))
    })
    chain := auth(handler)
    http.Handle("/api", chain)
    http.ListenAndServe(":8080", nil)
}
```

- **Логика**: Цепочка строится через композицию функций.

---
### Когда использовать в микросервисах?

- **Обработка запросов**: Middleware для проверки, логирования, лимитов.
- **Сложная бизнес-логика**: Разделение шагов (валидация -> обработка -> уведомление).
- **Фильтрация данных**: Например, цепочка фильтров для обработки событий.

**Когда НЕ использовать**:

- Простые сценарии, где достаточно одной функции.
- Высоконагруженные системы, где важна минимальная задержка.

---
### Итог

Chain of Responsibility — это паттерн, где запросы передаются по цепочке обработчиков, пока один из них не выполнит задачу. В Go его можно реализовать через интерфейсы или middleware. В микросервисах он полезен для гибкой обработки запросов или событий, обеспечивая высокую связность внутри обработчиков и низкую связанность между ними.




-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
