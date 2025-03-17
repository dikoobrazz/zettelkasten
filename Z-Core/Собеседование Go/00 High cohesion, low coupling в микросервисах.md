2025030701:51
___
Date: 07-03-2025 | 01:51
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## High cohesion | low coupling в микросервисах

> **High Cohesion** (высокая связность) и **Low Coupling** (низкая связанность) — это фундаментальные принципы проектирования программного обеспечения, которые особенно важны в микросервисной архитектуре. Они помогают создавать независимые, устойчивые и масштабируемые системы.

---
### Что такое High Cohesion?

**Определение**: Высокая связность означает, что элементы внутри одного модуля (или микросервиса) тесно связаны между собой и выполняют одну четко определенную задачу. Все части модуля работают на достижение общей цели, а не разбрасываются по разным функциям.

**Пример в контексте микросервисов**:

- Микросервис `UserService` должен заниматься только управлением пользователями (создание, обновление, получение данных), а не, например, обработкой заказов.

**Пример нарушения (низкая связность внутри сервиса)**:
```go
package main

type UserService struct{}

func (s *UserService) CreateUser(name string) {
    println("User created:", name)
}

func (s *UserService) ProcessOrder(orderID int) { // Чужая ответственность
    println("Order processed:", orderID)
}
```

- **Проблема**: `UserService` смешивает управление пользователями и заказами.

**Исправление (высокая связность)**:
```go
package users

type UserService struct{}

func (s *UserService) CreateUser(name string) {
    println("User created:", name)
}

package orders

type OrderService struct{}

func (s *OrderService) ProcessOrder(orderID int) {
    println("Order processed:", orderID)
}
```

- **Результат**: Каждый сервис отвечает за свою задачу.

---
### Что такое Low Coupling?

**Определение**: Низкая связанность означает, что модули (или микросервисы) имеют минимальную зависимость друг от друга. Изменение одного сервиса не должно ломать другой, а взаимодействие происходит через четко определенные интерфейсы (API, события).

**Пример в контексте микросервисов**:

- `UserService` и `OrderService` не вызывают методы друг друга напрямую, а используют HTTP-запросы или брокер сообщений (Kafka).

**Пример нарушения (высокая связанность)**:
```go
package main

type UserService struct{}

func (s *UserService) GetUser(id int) string {
    return "Alice"
}

type OrderService struct {
    userSvc *UserService // Прямая зависимость
}

func (s *OrderService) CreateOrder(userID int) {
    user := s.userSvc.GetUser(userID) // Жесткая связь
    println("Order for", user)
}
```

- **Проблема**: OrderService зависит от конкретной реализации UserService.

**Исправление (низкая связанность)**:
```go
package orders

type UserClient interface {
    GetUser(id int) (string, error)
}

type OrderService struct {
    userClient UserClient // Абстракция вместо конкретной реализации
}

func (s *OrderService) CreateOrder(userID int) {
    user, err := s.userClient.GetUser(userID)
    if err != nil {
        println("Failed to get user:", err)
        return
    }
    println("Order for", user)
}

package main

type HTTPUserClient struct {
    baseURL string
}

func (c *HTTPUserClient) GetUser(id int) (string, error) {
    resp, err := http.Get(fmt.Sprintf("%s/user/%d", c.baseURL, id))
    if err != nil {
        return "", err
    }
    defer resp.Body.Close()
    return "Alice", nil // Упрощение
}

func main() {
    client := &HTTPUserClient{baseURL: "http://localhost:8081"}
    svc := &OrderService{userClient: client}
    svc.CreateOrder(123)
}
```

- **Результат**: `OrderService` зависит только от интерфейса, а не от конкретного сервиса.

---
### Зачем это нужно в микросервисах?

#### High Cohesion

1. **Четкая ответственность**:
    - Каждый микросервис выполняет одну задачу (например, `UserService` — только пользователи).
    - Упрощает понимание и поддержку кода.
2. **Легкость масштабирования**:
    - Сервис с одной задачей проще масштабировать под конкретную нагрузку.
3. **Уменьшение побочных эффектов**:
    - Изменения в одной функции не затрагивают несвязанные части.

**Пример**: Если `UserService` отвечает только за пользователей, то баг в логике заказов не затронет его.

#### Low Coupling

1. **Независимость развертывания**:
    - Сервисы можно обновлять или переписывать без влияния на другие.
    - Пример: Обновление `UserService` не ломает `OrderService`.
2. **Устойчивость к сбоям**:
    - Сбой одного сервиса не вызывает каскадный эффект.
    - Пример: Если `UserService` недоступен, `OrderService` может использовать кэш.
3. **Гибкость технологий**:
    - Каждый сервис может использовать свой стек (Go, Python), если взаимодействие идет через API.

**Пример**: `OrderService` вызывает `UserService` через REST, а не напрямую, что позволяет заменить реализацию.

---
### Реализация в Go для микросервисов

#### High Cohesion

- Используйте пакеты для изоляции логики:
```text
user-service/
├── main.go
├── service/
│   └── user.go  // Только логика пользователей
```

#### Low Coupling

- Применяйте интерфейсы и асинхронное взаимодействие (Kafka):
```go
// OrderService отправляет событие в Kafka вместо прямого вызова
type OrderService struct {
    writer *kafka.Writer
}

func (s *OrderService) CreateOrder(userID int) error {
    return s.writer.WriteMessages(context.Background(),
        kafka.Message{
            Topic: "orders",
            Value: []byte(fmt.Sprintf("Order for user %d", userID)),
        },
    )
}
```

- **Результат**: `OrderService` не знает о `UserService`, а только отправляет событие.

---
### Практические советы для микросервисов

1. **Границы сервисов**:
    - Определяйте их по доменам (DDD), чтобы обеспечить высокую связность внутри и низкую между сервисами.
2. **API как контракт**:
    - Используйте REST, gRPC или сообщения для общения, избегая прямых зависимостей.
3. **Тестирование**:
    - Пишите модульные тесты для каждого сервиса отдельно:
```go
func TestOrderService(t *testing.T) {
    mockClient := &MockUserClient{}
    svc := &OrderService{userClient: mockClient}
    svc.CreateOrder(123)
}
```

4. **Мониторинг**:
    - Логируйте вызовы между сервисами для отладки.

---
### Итог

- **High Cohesion**: Делает микросервисы сфокусированными на одной задаче, упрощая поддержку и масштабирование.
- **Low Coupling**: Обеспечивает независимость сервисов, повышая устойчивость и гибкость системы.
- В Go это достигается через модули, интерфейсы и асинхронное общение (Kafka, HTTP).

Эти принципы — основа успешной микросервисной архитектуры.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
