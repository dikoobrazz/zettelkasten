2025030701:16
___
Date: 07-03-2025 | 01:16
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Event-Driven подход и Apache Kafka

> Пример микросервисной архитектуры с использованием **Event-Driven подхода** и Apache Kafka в Go. Мы создадим два микросервиса: один будет отправлять события (Producer), а второй — их принимать и обрабатывать (Consumer). Это будет реализация сценария, где `OrderService` отправляет событие о создании заказа, а `NotificationService` реагирует на него, отправляя уведомление.

---
### Предварительные требования

- Установлен Kafka (локально или в кластере). Для простоты можно использовать Docker:
```bash
docker run -d -p 9092:9092 --name kafka bitnami/kafka:latest
```

- Библиотека для Kafka в Go: github.com/segmentio/kafka-go.
```bash
go get github.com/segmentio/kafka-go
```

---
### Сценарий

1. `OrderService` создает заказ и отправляет событие `OrderCreated` в Kafka.
2. `NotificationService` слушает топик `orders` и обрабатывает событие, выводя уведомление.

---
### Код микросервисов

#### 1. OrderService (Producer)

**Структура**:
```text
order-service/
├── main.go
├── service/
│   └── service.go
```

**service/service.go**:
```go
package service

import (
    "context"
    "fmt"
    "github.com/segmentio/kafka-go"
)

type Order struct {
    ID     int
    UserID int
}

type Service struct {
    writer *kafka.Writer
}

func NewService(broker string) *Service {
    return &Service{
        writer: &kafka.Writer{
            Addr:     kafka.TCP(broker),
            Topic:    "orders",
            Balancer: &kafka.LeastBytes{},
        },
    }
}

func (s *Service) CreateOrder(userID int) (Order, error) {
    order := Order{ID: 123, UserID: userID} // ID для примера фиксирован
    msg := kafka.Message{
        Key:   []byte(fmt.Sprintf("order-%d", order.ID)),
        Value: []byte(fmt.Sprintf("Order %d created for user %d", order.ID, order.UserID)),
    }
    err := s.writer.WriteMessages(context.Background(), msg)
    if err != nil {
        return Order{}, fmt.Errorf("failed to send event: %v", err)
    }
    return order, nil
}

func (s *Service) Close() {
    s.writer.Close()
}
```

**main.go**:
```go
package main

import (
    "log"
    "order-service/service"
    "os"
    "os/signal"
)

func main() {
    broker := "localhost:9092" // Адрес Kafka
    svc := service.NewService(broker)
    defer svc.Close()

    // Пример создания заказа
    order, err := svc.CreateOrder(456)
    if err != nil {
        log.Fatalf("Failed to create order: %v", err)
    }
    log.Printf("Order created: %+v", order)

    // Ожидание сигнала для graceful shutdown
    sigChan := make(chan os.Signal, 1)
    signal.Notify(sigChan, os.Interrupt)
    <-sigChan
    log.Println("Shutting down OrderService")
}
```

---
#### 2. NotificationService (Consumer)

**Структура**:
```text
notification-service/
├── main.go
├── service/
│   └── service.go
```

**service/service.go**:
```go
package service

import (
    "context"
    "log"
    "github.com/segmentio/kafka-go"
)

type Service struct {
    reader *kafka.Reader
}

func NewService(broker string) *Service {
    return &Service{
        reader: kafka.NewReader(kafka.ReaderConfig{
            Brokers:  []string{broker},
            Topic:    "orders",
            GroupID:  "notification-group", // Для consumer group
            MinBytes: 10e3,                 // 10KB
            MaxBytes: 10e6,                 // 10MB
        }),
    }
}

func (s *Service) Listen(ctx context.Context) {
    for {
        msg, err := s.reader.ReadMessage(ctx)
        if err != nil {
            log.Printf("Error reading message: %v", err)
            continue
        }
        log.Printf("Received event: key=%s, value=%s", msg.Key, msg.Value)
        // Здесь можно добавить логику отправки уведомления (email, SMS и т.д.)
    }
}

func (s *Service) Close() {
    s.reader.Close()
}
```

**main.go**:
```go
package main

import (
    "context"
    "log"
    "notification-service/service"
    "os"
    "os/signal"
)

func main() {
    broker := "localhost:9092"
    svc := service.NewService(broker)
    defer svc.Close()

    ctx, cancel := context.WithCancel(context.Background())
    go svc.Listen(ctx)

    // Graceful shutdown
    sigChan := make(chan os.Signal, 1)
    signal.Notify(sigChan, os.Interrupt)
    <-sigChan
    log.Println("Shutting down NotificationService")
    cancel()
}
```

---
### Как это работает?

1. Запустите Kafka (например, через Docker).
2. Скомпилируйте и запустите оба сервиса:

```bash
cd order-service && go run .
cd notification-service && go run .
```

3. `OrderService` отправляет событие в топик `orders`.
4. `NotificationService` читает событие и выводит сообщение.

**Вывод OrderService**:
```text
2025/03/06 12:00:00 Received event: key=order-123, value=Order 123 created for user 456
```

---
### Архитектурные особенности

1. **Event-Driven подход**:
    - Сервисы общаются асинхронно через Kafka.
    - `OrderService` — producer, `NotificationService` — consumer.
2. **Изоляция**:
    - Каждый сервис независим, может масштабироваться отдельно.
    - Нет прямых HTTP/gRPC-вызовов, только события.
3. **Graceful Shutdown**:
    - Оба сервиса корректно завершают работу при SIGINT (Ctrl+C).
4. **Масштабируемость**:
    - Kafka автоматически распределяет сообщения между consumers 
	    в одной группе (`GroupID`).

---
### Расширения

- **Модель события**: Добавьте JSON-сериализацию для сложных данных:
```go
type OrderEvent struct {
    OrderID int `json:"order_id"`
    UserID  int `json:"user_id"`
}
```

- **[[00 Ретраи (retries) и backoff|Ретраи]] и отказоустойчивость**: Используйте **[[00 Ретраи (retries) и backoff|backoff]]** для повторных попыток при сбоях Kafka.
- **Мониторинг**: Добавьте Prometheus для метрик обработки сообщений.

---
### Итог

Этот пример демонстрирует базовую Event-Driven архитектуру микросервисов в Go с Kafka:

- `OrderService` отправляет события о создании заказа.
- `NotificationService` слушает и реагирует на них.
- Kafka обеспечивает асинхронность и надежность доставки.

Это подходит для систем с высокой нагрузкой и требованиями к асинхронной обработке (например, e-commerce, логистика).

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Ретраи (retries) и backoff]]

------
**Links** (Внешние ссылки)
-
