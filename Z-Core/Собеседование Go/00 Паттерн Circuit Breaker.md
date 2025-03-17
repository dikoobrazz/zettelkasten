2025030702:30
___
Date: 07-03-2025 | 02:30
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Паттерн Circuit Breaker

> **Circuit Breaker** (размыкатель цепи) — это паттерн проектирования, используемый в распределенных системах, таких как микросервисы, для повышения отказоустойчивости. Он предотвращает перегрузку системы, когда один из её компонентов (например, внешний сервис) становится недоступным или работает с ошибками.

---
### Что такое Circuit Breaker?

**Определение**: Circuit Breaker действует как "предохранитель" в электрической цепи. Он отслеживает состояние зависимости (например, внешнего сервиса) и при определенном количестве сбоев "размыкает цепь", временно блокируя запросы к этой зависимости. После паузы он пытается восстановить соединение.

**Аналогия**:

- Если выключатель в доме срабатывает из-за перегрузки, он отключает электричество, чтобы предотвратить пожар. Circuit Breaker делает то же самое в софте, защищая систему от каскадных сбоев.

**Состояния Circuit Breaker**:

1. **Closed (закрыт)**: Запросы проходят нормально. Ошибки подсчитываются.
2. **Open (открыт)**: После превышения порога ошибок запросы блокируются, возвращается ошибка без обращения к сервису.
3. **Half-Open (полуоткрыт)**: После таймаута Circuit Breaker пропускает тестовый запрос. Если он успешен, переходит в Closed, иначе — обратно в Open.

---
### Как работает Circuit Breaker?

1. **Мониторинг**: Отслеживает успешные и неуспешные вызовы.
2. **Порог**: Если процент ошибок или число сбоев превышает заданный лимит (например, 5 ошибок за 10 секунд), цепь размыкается.
3. **Ожидание**: В состоянии Open запросы не отправляются, а сразу возвращается ошибка (или fallback).
4. **Попытка восстановления**: Через заданное время (например, 30 секунд) цепь переходит в Half-Open для проверки.

**Пример сценария**:

- OrderService вызывает PaymentService.
- PaymentService начинает отвечать ошибками из-за перегрузки.
- Circuit Breaker размыкает цепь, и OrderService перестает его вызывать, используя кэш или возвращая сообщение "Платежи временно недоступны".

---
### Когда использовать Circuit Breaker?

1. **Внешние зависимости**:
    - Вызовы к сторонним API (например, платежные шлюзы).
    - Пример: Если Stripe API недоступен, не нужно бомбардировать его запросами.
2. **Микросервисы**:
    - Когда один сервис зависит от другого, и сбой не должен привести к каскаду.
    - Пример: `OrderService` -> `UserService`.
3. **Сетевые операции**:
    - Таймауты, обрывы соединений, перегрузка сети.
4. **Защита ресурсов**:
    - Предотвращение исчерпания потоков или памяти из-за бесконечных ретраев.

**Когда НЕ использовать**:

- Для критически важных операций, где каждая попытка обязательна (лучше использовать ретраи с backoff).
- В простых системах без распределенных зависимостей.

---
### Пример реализации в Go

Используем библиотеку github.com/sony/gobreaker:
```bash
go get github.com/sony/gobreaker
```

**Код**:
```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "time"
    "github.com/sony/gobreaker"
)

var cb *gobreaker.CircuitBreaker

func init() {
    // Настройка Circuit Breaker
    cb = gobreaker.NewCircuitBreaker(gobreaker.Settings{
        Name:        "PaymentService",        // Имя для логов
        MaxRequests: 2,                       // Кол-во запросов в Half-Open
        Interval:    10 * time.Second,        // Период сброса статистики в Closed
        Timeout:     5 * time.Second,         // Время в Open перед Half-Open
        ReadyToTrip: func(counts gobreaker.Counts) bool {
            return counts.ConsecutiveFailures > 3 // Размыкаем после 3 ошибок подряд
        },
        OnStateChange: func(name string, from, to gobreaker.State) {
            log.Printf("Circuit Breaker %s changed from %v to %v", name, from, to)
        },
    })
}

func callPaymentService() (string, error) {
    result, err := cb.Execute(func() (interface{}, error) {
        resp, err := http.Get("http://localhost:8081/pay") // Симуляция внешнего сервиса
        if err != nil {
            return nil, fmt.Errorf("payment failed: %v", err)
        }
        defer resp.Body.Close()
        return "Payment successful", nil
    })
    if err != nil {
        return "Payment unavailable (circuit open)", err
    }
    return result.(string), nil
}

func main() {
    for i := 0; i < 10; i++ {
        result, err := callPaymentService()
        log.Printf("Result: %s, Error: %v", result, err)
        time.Sleep(1 * time.Second)
    }
}
```

- **Логика**:
    - Если `/pay` недоступен, после 3 сбоев цепь размыкается.
    - Через 5 секунд (Timeout) Circuit Breaker переходит в Half-Open и проверяет сервис.

**Вывод (если сервис недоступен)**:
```text
Result: payment failed: ..., Error: ...
Result: payment failed: ..., Error: ...
Result: payment failed: ..., Error: ...
Circuit Breaker PaymentService changed from Closed to Open
Result: Payment unavailable (circuit open), Error: circuit breaker is open
...
Circuit Breaker PaymentService changed from Open to Half-Open
```

---
### Реализация с fallback

Добавим запасной вариант:
```go
func callPaymentServiceWithFallback() (string, error) {
    result, err := cb.Execute(func() (interface{}, error) {
        resp, err := http.Get("http://localhost:8081/pay")
        if err != nil {
            return nil, err
        }
        defer resp.Body.Close()
        return "Payment successful", nil
    })
    if err != nil {
        // Fallback: использовать кэш или сообщение
        return "Payment queued (will retry later)", nil
    }
    return result.(string), nil
}
```

- **Результат**: Вместо ошибки пользователю возвращается осмысленный ответ.

---
### В каких случаях использовать в микросервисах?

1. **Вызовы между сервисами**:
    - `OrderService` -> `PaymentService`: Если платежный сервис упал, заказы не блокируются полностью.
2. **Интеграция с внешними API**:
    - Вызовы к погодному API, где временные сбои нормальны.
3. **Ограничение нагрузки**:
    - Защита от перегрузки базы данных или стороннего сервиса.

**Пример в микросервисах с Kafka**:
```go
func sendToKafka() error {
    _, err := cb.Execute(func() (interface{}, error) {
        return nil, writer.WriteMessages(context.Background(), kafka.Message{Value: []byte("event")})
    })
    if err != nil {
        return fmt.Errorf("Kafka unavailable, event dropped: %v", err)
    }
    return nil
}
```

- **Сценарий**: Если Kafka недоступен, Circuit Breaker предотвращает бесконечные попытки.

---
### Плюсы и минусы

**Плюсы**:

- Уменьшает каскадные сбои.
- Защищает ресурсы (потоки, память).
- Упрощает восстановление системы.

**Минусы**:

- Усложняет код.
- Требует настройки (пороги, таймауты).
- Может скрыть проблему, если fallback неадекватен.

---
### Советы для Go

- Используйте `gobreaker` или `eapache/go-resiliency` для готовых решений.
- Настройте пороги на основе статистики (например, 95-й перцентиль задержек).
- Логируйте состояния для мониторинга.

---

### Итог

Circuit Breaker — это паттерн для управления сбоями в распределенных системах. Он "размыкает цепь" при проблемах, предотвращая перегрузку, и используется в микросервисах для вызовов к внешним сервисам или зависимостям. В Go его легко реализовать с библиотеками вроде `gobreaker`, добавляя отказоустойчивость и гибкость.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
