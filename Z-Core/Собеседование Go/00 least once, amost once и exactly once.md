2025030715:22
___
Date: 07-03-2025 | 15:22
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## least once, amost once и exactly once

> В контексте систем обмена сообщениями (например, Apache Kafka, RabbitMQ) и микросервисов понятия **at least once**, **at most once** и **exactly once** описывают **гарантии доставки сообщений**. Эти термины определяют, как брокер сообщений и потребители (consumers) обрабатывают данные, и имеют прямое влияние на надежность, производительность и сложность системы.

---
### Основные концепции

Гарантии доставки связаны с тем, как сообщения передаются от producer'а к consumer'у через брокер:

1. **At Most Once**: Сообщение доставляется не более одного раза (может быть потеряно).
2. **At Least Once**: Сообщение доставляется хотя бы один раз (может быть дублировано).
3. **Exactly Once**: Сообщение доставляется ровно один раз (без потерь и дубликатов).

Эти гарантии зависят от конфигурации producer'а, брокера и consumer'а, а также от обработки ошибок (например, сбоев сети или перезапуска).

___
### 1. At Most Once (Не более одного раза)

**Описание**: Сообщение отправляется и обрабатывается максимум один раз. Если что-то идет не так (например, сбой сети), сообщение теряется, и повторной попытки не происходит.

**Как работает**:

- Producer отправляет сообщение и не ждет подтверждения (fire-and-forget).
- Consumer обрабатывает сообщение без подтверждения брокеру.
- Если сбой происходит до или во время обработки, сообщение не восстанавливается.

**Пример в Go (Kafka)**:
```go
package main

import (
    "context"
    "github.com/segmentio/kafka-go"
)

func main() {
    writer := kafka.NewWriter(kafka.WriterConfig{
        Brokers:      []string{"localhost:9092"},
        Topic:        "orders",
        BatchSize:    1, // Отправка сразу
        Async:        true, // Без ожидания подтверждения
    })
    writer.WriteMessages(context.Background(), kafka.Message{Value: []byte("Order 1")})
    writer.Close() // Не ждем доставки
}
```

**Consumer**:
```go
reader := kafka.NewReader(kafka.ReaderConfig{
    Brokers: []string{"localhost:9092"},
    Topic:   "orders",
})
msg, _ := reader.ReadMessage(context.Background())
println(string(msg.Value)) // Без подтверждения offset
reader.Close()
```

**Когда использовать**:

- Потери допустимы (например, некритичные логи или метрики).
- Важна максимальная скорость.

**Плюсы**:

- Высокая производительность (нет overhead на подтверждения).
- Простота реализации.

**Минусы**:

- Возможна потеря сообщений (например, при сбое сети или consumer'а).

---
### 2. At Least Once (Хотя бы один раз)

**Описание**: Сообщение гарантированно доставляется хотя бы один раз, но может быть обработано несколько раз (дублирование), если consumer не подтверждает обработку корректно.

**Как работает**:

- Producer ждет подтверждения от брокера (ack).
- Consumer обрабатывает сообщение и подтверждает offset, но при сбое до подтверждения может перечитать сообщение.
- Брокер сохраняет сообщение, пока не получит подтверждение.

**Пример в Go (Kafka)**:
```go
package main

import (
    "context"
    "github.com/segmentio/kafka-go"
)

func main() {
    // Producer с подтверждением
    writer := kafka.NewWriter(kafka.WriterConfig{
        Brokers:  []string{"localhost:9092"},
        Topic:    "orders",
        RequiredAcks: -1, // Ждать подтверждения от всех реплик
    })
    writer.WriteMessages(context.Background(), kafka.Message{Value: []byte("Order 1")})
    writer.Close()

    // Consumer
    reader := kafka.NewReader(kafka.ReaderConfig{
        Brokers: []string{"localhost:9092"},
        Topic:   "orders",
        GroupID: "order-group",
    })
    for {
        msg, _ := reader.FetchMessage(context.Background()) // Читаем без коммита
        println(string(msg.Value))
        reader.CommitMessages(context.Background(), msg) // Подтверждаем после обработки
    }
}
```

- **Логика**:
    - Producer: `RequiredAcks: -1` гарантирует доставку в брокер.
    - Consumer: Если сбой после `println`, но до `CommitMessages`, сообщение будет перечитано.

**Когда использовать**:

- Потери недопустимы, но дубликаты приемлемы (например, отправка уведомлений).
- Простота важнее строгой уникальности.

**Плюсы**:

- Гарантия доставки.
- Средняя сложность.

**Минусы**:

- Возможны дубликаты (например, при сбое consumer'а).

---
### 3. Exactly Once (Ровно один раз)

**Описание**: Сообщение доставляется и обрабатывается ровно один раз — без потерь и дубликатов. Это наиболее сложная гарантия, требующая транзакций или идемпотентности.

**Как работает**:

- Producer использует транзакции или уникальные идентификаторы (idempotency).
- Consumer подтверждает обработку атомарно с записью результата.
- Брокер поддерживает механизм дедупликации.

**Пример в Go (Kafka с транзакциями)**:
```go
package main

import (
    "context"
    "github.com/segmentio/kafka-go"
)

func main() {
    // Producer с транзакциями
    writer := kafka.NewWriter(kafka.WriterConfig{
        Brokers:          []string{"localhost:9092"},
        Topic:            "orders",
        TransactionalID:  "tx-1", // Уникальный ID транзакции
    })
    writer.BeginTransaction(context.Background())
    writer.WriteMessages(context.Background(), kafka.Message{Value: []byte("Order 1")})
    writer.CommitTransaction(context.Background())
    writer.Close()

    // Consumer с идемпотентностью
    reader := kafka.NewReader(kafka.ReaderConfig{
        Brokers: []string{"localhost:9092"},
        Topic:   "orders",
        GroupID: "order-group",
    })
    processed := make(map[string]bool) // Храним обработанные ID
    for {
        msg, _ := reader.FetchMessage(context.Background())
        msgID := string(msg.Key) // Уникальный ID сообщения
        if !processed[msgID] {
            println(string(msg.Value))
            processed[msgID] = true
            reader.CommitMessages(context.Background(), msg)
        }
    }
}
```

- **Логика**:
    - Producer: Транзакция гарантирует, что сообщение записано ровно раз.
    - Consumer: Проверка `msgID` предотвращает дублирование.

**Когда использовать**:

- Дубликаты и потери критичны (например, финансовые транзакции).
- Нужна строгая консистентность.

**Плюсы**:

- Максимальная надежность.
- Нет дубликатов и потерь.

**Минусы**:

- Сложность реализации (транзакции, идемпотентность).
- Низкая производительность из-за overhead.

---
### Отличия в деталях

|**Гарантия**|**Доставка**|**Дубликаты**|**Потери**|**Сложность**|**Производительность**|
|---|---|---|---|---|---|
|**At Most Once**|Не более 1 раза|Нет|Возможны|Низкая|Высокая|
|**At Least Once**|Хотя бы 1 раз|Возможны|Нет|Средняя|Средняя|
|**Exactly Once**|Ровно 1 раз|Нет|Нет|Высокая|Низкая|

---
### Как это работает в микросервисах?

1. **At Most Once**:
    - Пример: Логирование метрик (`MetricsService` отправляет данные, потеря некритична).
    - Настройка: Отключить подтверждения (`acks=0` в Kafka).
2. **At Least Once**:
    - Пример: Отправка email (`OrderService` -> `EmailService`, повторная отправка допустима).
    - Настройка: Включить подтверждения и коммит после обработки.
3. **Exactly Once**:
    - Пример: Обработка платежей (`PaymentService` -> `BankService`, дубликаты недопустимы).
    - Настройка: Транзакции в Kafka или идемпотентность на стороне consumer'а.

---
### Практические советы для Go

1. **At Most Once**:
    - Используйте `Async: true` в Kafka или минимальные подтверждения в RabbitMQ.
    - Подходит для некритичных данных.
2. **At Least Once**:
    - Подтверждайте offset после успешной обработки (`CommitMessages`).
    - Добавьте ретраи для надежности.
3. **Exactly Once**:
    - Используйте транзакции в Kafka (с TransactionalID).
    - Реализуйте идемпотентность с уникальными ID сообщений:
```go
if !db.Exists(msgID) {
    processMessage(msg)
    db.MarkProcessed(msgID)
}
```

---
### Итог

- **At Most Once**: Быстро, но с потерями — для некритичных задач.
- **At Least Once**: Надежно, но с дубликатами — для большинства асинхронных операций.
- **Exactly Once**: Сложно, но идеально — для строгой консистентности.

Выбор зависит от требований к надежности и производительности. В микросервисах чаще используют **at least once** с идемпотентностью для баланса.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
