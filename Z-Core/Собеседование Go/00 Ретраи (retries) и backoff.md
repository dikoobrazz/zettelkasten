2025030701:28
___
Date: 07-03-2025 | 01:28
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## **Ретраи (retries)** и **backoff**

> **Ретраи** (**retries**) и **backoff** — это техники, используемые в программировании для повышения отказоустойчивости систем, особенно в распределенных архитектурах, таких как микросервисы. Они помогают справляться с временными сбоями (например, сетевыми ошибками или недоступностью сервиса).

---
### Что такое ретраи (Retries)?

**Определение**: Ретраи — это повторные попытки выполнить операцию, которая завершилась неудачей, в надежде, что проблема временная и следующая попытка будет успешной.

**Когда использовать**:

- Сетевые запросы (HTTP, gRPC) к внешним сервисам.
- Операции с базами данных или брокерами сообщений (Kafka, RabbitMQ).
- Временные сбои (timeout, сервер перегружен).

**Пример без ретраев**:
```go
func CallService() error {
    resp, err := http.Get("http://example.com")
    if err != nil {
        return fmt.Errorf("request failed: %v", err)
    }
    defer resp.Body.Close()
    return nil
}
```

Если запрос не удался (например, из-за таймаута), операция просто завершится ошибкой.

**Пример с ретраями**:
```go
func CallServiceWithRetries() error {
    maxAttempts := 3
    for attempt := 1; attempt <= maxAttempts; attempt++ {
        resp, err := http.Get("http://example.com")
        if err == nil {
            defer resp.Body.Close()
            return nil
        }
        log.Printf("Attempt %d failed: %v", attempt, err)
        if attempt == maxAttempts {
            return fmt.Errorf("all %d attempts failed: %v", maxAttempts, err)
        }
        time.Sleep(1 * time.Second) // Простая задержка перед следующей попыткой
    }
    return nil
}
```

- **Логика**: Пробуем 3 раза. Если все попытки неудачны, возвращаем ошибку.

**Проблема**: Постоянные попытки с фиксированной задержкой могут перегрузить сервис, если он уже испытывает проблемы.

---
### Что такое Backoff?

**Определение**: Backoff — это стратегия увеличения интервала между повторными попытками, чтобы дать системе время восстановиться и избежать чрезмерной нагрузки. Вместо фиксированной задержки (как в примере выше) время ожидания растет с каждой попыткой.

**Типы backoff**:

1. **Фиксированный (Fixed)**: Постоянная задержка (например, 1 секунда).
2. **Экспоненциальный (Exponential)**: Удвоение времени (1с, 2с, 4с...).
3. **С джиттером (Jitter)**: Экспоненциальный с добавлением случайного разброса, чтобы избежать одновременных запросов от множества клиентов.

**Когда использовать**:

- При временных сбоях, где повторные попытки через равные интервалы неэффективны.
- Для защиты сервиса от "лавины запросов" (thundering herd problem).

---
### Пример с экспоненциальным backoff в Go

**Без библиотеки**:
```go
package main

import (
    "fmt"
    "log"
    "math"
    "net/http"
    "time"
)

func CallServiceWithExponentialBackoff() error {
    maxAttempts := 5
    baseDelay := 100 * time.Millisecond // Начальная задержка

    for attempt := 1; attempt <= maxAttempts; attempt++ {
        resp, err := http.Get("http://example.com")
        if err == nil {
            defer resp.Body.Close()
            return nil
        }
        log.Printf("Attempt %d failed: %v", attempt, err)
        if attempt == maxAttempts {
            return fmt.Errorf("all %d attempts failed: %v", maxAttempts, err)
        }

        // Экспоненциальная задержка: baseDelay * 2^(attempt-1)
        delay := baseDelay * time.Duration(math.Pow(2, float64(attempt-1)))
        log.Printf("Waiting %v before next attempt", delay)
        time.Sleep(delay)
    }
    return nil
}

func main() {
    if err := CallServiceWithExponentialBackoff(); err != nil {
        log.Fatal(err)
    }
    log.Println("Success!")
}
```

**Вывод (если сервис недоступен)**:
```text
Attempt 1 failed: ...
Waiting 100ms before next attempt
Attempt 2 failed: ...
Waiting 200ms before next attempt
Attempt 3 failed: ...
Waiting 400ms before next attempt
Attempt 4 failed: ...
Waiting 800ms before next attempt
Attempt 5 failed: ...
all 5 attempts failed: ...
```

---
### Пример с backoff и джиттером (используя библиотеку)

Для удобства можно использовать библиотеку github.com/cenkalti/backoff/v4:
```bash
go get github.com/cenkalti/backoff/v4
```

**Код**:
```go
package main

import (
    "log"
    "net/http"
    "time"
    "github.com/cenkalti/backoff/v4"
)

func CallServiceWithBackoff() error {
    operation := func() error {
        resp, err := http.Get("http://example.com")
        if err != nil {
            return err
        }
        defer resp.Body.Close()
        return nil
    }

    // Экспоненциальный backoff с джиттером
    b := backoff.NewExponentialBackOff()
    b.InitialInterval = 100 * time.Millisecond // Начальная задержка
    b.MaxInterval = 5 * time.Second            // Максимальная задержка
    b.MaxElapsedTime = 15 * time.Second        // Общее время попыток

    err := backoff.Retry(func() error {
        err := operation()
        if err != nil {
            log.Printf("Failed, retrying: %v", err)
        }
        return err
    }, b)

    if err != nil {
        return fmt.Errorf("failed after retries: %v", err)
    }
    return nil
}

func main() {
    if err := CallServiceWithBackoff(); err != nil {
        log.Fatal(err)
    }
    log.Println("Success!")
}
```

- **Логика**:
    - Начинает с 100 мс, увеличивает задержку экспоненциально до 5 секунд.
    - Добавляет случайный джиттер для избежания одновременных запросов.
    - Прекращает после 15 секунд общего времени.

**Вывод (примерный)**:
```text
Failed, retrying: ...
Failed, retrying: ... (через ~150ms)
Failed, retrying: ... (через ~300ms)
...
failed after retries: ...
```

---
### Ретраи и Backoff в микросервисах с Kafka

В контексте Kafka (из предыдущего примера) ретраи с backoff полезны для обработки временных сбоев брокера:
```go
func (s *Service) CreateOrder(userID int) (Order, error) {
    order := Order{ID: 123, UserID: userID}
    msg := kafka.Message{
        Key:   []byte(fmt.Sprintf("order-%d", order.ID)),
        Value: []byte(fmt.Sprintf("Order %d created for user %d", order.ID, order.UserID)),
    }

    b := backoff.NewExponentialBackOff()
    b.MaxElapsedTime = 10 * time.Second

    err := backoff.Retry(func() error {
        return s.writer.WriteMessages(context.Background(), msg)
    }, b)

    if err != nil {
        return Order{}, fmt.Errorf("failed to send event after retries: %v", err)
    }
    return order, nil
}
```

- **Сценарий**: Если Kafka-брокер временно недоступен, сервис пытается отправить сообщение с увеличивающейся задержкой.

---
### Ключевые моменты

1. **Ретраи**:
    - Определите максимальное число попыток.
    - Проверяйте, стоит ли повторять (например, не для ошибок 400 Bad Request).
2. **Backoff**:
    - Используйте экспоненциальный рост для снижения нагрузки.
    - Добавляйте джиттер в распределенных системах.
3. **Ограничения**:
    - Установите максимальное время (`MaxElapsedTime`), чтобы не зависнуть надолго.
    - Логируйте попытки для отладки.
4. **Инструменты в Go**:
    - `time.Sleep` — простейший вариант.
    - `cenkalti/backoff` — мощная библиотека с готовыми стратегиями.

---

### Итог

- **Ретраи** — повторение операций при сбоях.
- **Backoff** — стратегия задержек между попытками, часто экспоненциальная с джиттером.
- В Go это легко реализуется вручную или с библиотеками, такими как `backoff/v4`. В микросервисах с Kafka они помогают справляться с временными проблемами брокера или сети.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
