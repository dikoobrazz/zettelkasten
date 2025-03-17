2025030421:03
___
Date: 04-03-2025 | 21:03
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Паттерн Saga

> **Паттерн Saga** — это архитектурный подход к управлению распределенными транзакциями в системах, где традиционные [[00 Что такое ACID|ACID]]-транзакции (атомарность, согласованность, изолированность, долговечность) трудно реализовать, например, в микросервисах.

---
### Что такое паттерн Saga?
#### Определение

- **Saga** — это последовательность локальных транзакций, каждая из которых выполняется в отдельной системе (например, микросервисе), с механизмом компенсации для отката при сбое.
- Впервые описан в статье 1987 года "Sagas" авторов Hector Garcia-Molina и Kenneth Salem как альтернатива длинным транзакциям в базах данных.
#### Зачем нужен?

- В распределенных системах (микросервисы, базы данных) невозможно использовать глобальную блокировку или двухфазный коммит (2PC) из-за масштаба и задержек.
- Saga разбивает сложную операцию на шаги, которые можно выполнить независимо, с возможностью отката при ошибке.
#### Основной принцип

- Вместо одной большой транзакции: T=$T1$⋅$T2$⋅...⋅$Tn$
- Если шаг $Ti$​ *fails*, выполняются компенсации $Ci−1$,$Ci−2$,...,$C1$ для отмены предыдущих шагов.

---
### Типы Saga

1. **Хореография (Choreography)**:
    
    - **Суть**: Каждый сервис сам решает, что делать, реагируя на события от других сервисов (без центрального управления).
    - **Как работает**: Сервисы обмениваются сообщениями (например, через очереди типа Kafka), выполняя шаги и компенсации по событиям.
    - **Плюсы**: Децентрализация, гибкость.
    - **Минусы**: Сложно отлаживать, нет единой точки контроля.
    
    **Пример**: Заказ в интернет-магазине:
    
    - Сервис заказа → "Заказ создан".
    - Сервис оплаты → "Оплата прошла" или "Оплата отклонена" → компенсировать заказ.
    
2. **Оркестрация (Orchestration)**:
    
    - **Суть**: Центральный координатор (оркестратор) управляет шагами, вызывая сервисы и обрабатывая ошибки.
    - **Как работает**: Оркестратор отправляет команды сервисам и следит за выполнением/компенсацией.
    - **Плюсы**: Легче отслеживать состояние, централизованное управление.
    - **Минусы**: Оркестратор — точка отказа, больше зависимости.
    
    **Пример**: Оркестратор вызывает:
    
    - Сервис заказа → "Создать".
    - Сервис оплаты → "Оплатить".
    - При сбое: "Отменить заказ".

---
### Как работает Saga?

1. **Прямой путь (Forward Path)**:
    - Выполняются шаги $T1$,$T2$,...,`Tn`
    - Каждый шаг сохраняет данные локально и публикует событие/команду для следующего.
2. **Компенсация (Compensation)**:
    - Если шаг $Ti$ *fails*, выполняются компенсационные действия $Ci−1$,...,$C1$.
    - Компенсации должны быть идемпотентными (повторное выполнение не ломает систему).
3. **Согласованность**:
    - Saga обеспечивает ** eventual consistency** (конечную согласованность), а не мгновенную (как в ACID).

---
### Пример реализации в Go (Оркестрация)

#### Сценарий

- Интернет-магазин: создание заказа, списание денег, резервирование товара.
- Если что-то идет не так, откатываем выполненные шаги.
##### Код с комментариями
```go
package main

import (
    "errors"
    "fmt"
    "time"
)

// OrderService — сервис управления заказами
type OrderService struct {
    orders map[int]bool // имитация базы заказов
}

func (s *OrderService) CreateOrder(id int) error {
    fmt.Printf("Создание заказа %d\n", id)
    s.orders[id] = true // заказ создан
    return nil
}

func (s *OrderService) CancelOrder(id int) error {
    fmt.Printf("Отмена заказа %d\n", id)
    delete(s.orders, id) // удаляем заказ
    return nil
}

// PaymentService — сервис оплаты
type PaymentService struct {
    payments map[int]int // имитация базы платежей
}

func (s *PaymentService) ProcessPayment(id, amount int) error {
    fmt.Printf("Обработка платежа для заказа %d на сумму %d\n", id, amount)
    if amount <= 0 { // имитация ошибки
        return errors.New("неверная сумма платежа")
    }
    s.payments[id] = amount
    return nil
}

func (s *PaymentService) RefundPayment(id int) error {
    fmt.Printf("Возврат платежа для заказа %d\n", id)
    delete(s.payments, id)
    return nil
}

// InventoryService — сервис инвентаря
type InventoryService struct {
    stock map[int]int // имитация склада
}

func (s *InventoryService) ReserveStock(id, quantity int) error {
    fmt.Printf("Резервирование %d единиц товара для заказа %d\n", quantity, id)
    s.stock[id] = quantity
    return nil
}

func (s *InventoryService) ReleaseStock(id int) error {
    fmt.Printf("Освобождение товара для заказа %d\n", id)
    delete(s.stock, id)
    return nil
}

// Saga — структура оркестратора
type Saga struct {
    orderSvc    *OrderService
    paymentSvc  *PaymentService
    invSvc      *InventoryService
}

func NewSaga() *Saga {
    return &Saga{
        orderSvc:   &OrderService{orders: make(map[int]bool)},
        paymentSvc: &PaymentService{payments: make(map[int]int)},
        invSvc:     &InventoryService{stock: make(map[int]int)},
    }
}

// Execute — выполнение саги
func (s *Saga) Execute(orderID int, amount, quantity int) error {
    steps := []struct {
        action   func() error // прямой шаг
        rollback func() error // компенсация
    }{
        {
            action:   func() error { return s.orderSvc.CreateOrder(orderID) },
            rollback: func() error { return s.orderSvc.CancelOrder(orderID) },
        },
        {
            action:   func() error { return s.paymentSvc.ProcessPayment(orderID, amount) },
            rollback: func() error { return s.paymentSvc.RefundPayment(orderID) },
        },
        {
            action:   func() error { return s.invSvc.ReserveStock(orderID, quantity) },
            rollback: func() error { return s.invSvc.ReleaseStock(orderID) },
        },
    }

    // выполняем шаги саги
    for i, step := range steps {
        if err := step.action(); err != nil {
            fmt.Printf("Ошибка на шаге %d: %v\n", i, err)
            // выполняем компенсации для предыдущих шагов
            for j := i - 1; j >= 0; j-- {
                if err := steps[j].rollback(); err != nil {
                    fmt.Printf("Ошибка компенсации на шаге %d: %v\n", j, err)
                }
            }
            return err
        }
    }
    return nil
}

func main() {
    saga := NewSaga()

    // Успешная сага
    fmt.Println("Успешная транзакция:")
    err := saga.Execute(1, 100, 2)
    if err != nil {
        fmt.Println("Ошибка:", err)
    } else {
        fmt.Println("Транзакция завершена успешно")
    }

    time.Sleep(1 * time.Second)
    fmt.Println("\nТранзакция с ошибкой:")
    // Сага с ошибкой (неверная сумма платежа)
    err = saga.Execute(2, -10, 3)
    if err != nil {
        fmt.Println("Ошибка:", err)
    }
}
```

**Вывод**:
```text
Успешная транзакция:
Создание заказа 1
Обработка платежа для заказа 1 на сумму 100
Резервирование 2 единиц товара для заказа 1
Транзакция завершена успешно

Транзакция с ошибкой:
Создание заказа 2
Обработка платежа для заказа 2 на сумму -10
Ошибка на шаге 1: неверная сумма платежа
Отмена заказа 2
Ошибка: неверная сумма платежа
```

---
### Объяснение кода

#### Структура

- **Сервисы**: `OrderService`, `PaymentService`, `InventoryService` — имитация микросервисов.
- **Saga**: Оркестратор с массивом шагов, каждый из которых имеет:
    - `action`: Выполнение шага.
    - `rollback`: Компенсация при сбое.
#### Логика

- **Прямой путь**: Вып. шаги последовательно `CreateOrder → ProcessPayment → ReserveStock`
- **Компенсация**: Если шаг *fails* (например, `ProcessPayment` с -`10`), откатываем предыдущие шаги в обратном порядке.
- **Ошибки**: Возвращаем первую ошибку, логируя проблемы компенсации.

---
### Тонкости использования

1. **Идемпотентность**:
    - Компенсации должны быть идемпотентными (повторный вызов не ломает систему).
    - Пример: `CancelOrder` безопасно вызывается даже для несуществующего заказа.
2. **Состояние**:
    - Оркестратор должен сохранять состояние саги (например, в базе), чтобы возобновить при сбое.
3. **Асинхронность**:
    - В реальных системах шаги выполняются асинхронно через очереди (Kafka, RabbitMQ), а не напрямую.
4. **Отказоустойчивость**:
    - Компенсации могут провалиться — нужно логировать и обрабатывать такие случаи.

---
### Пример реализации в Go (Хореография)

#### ### Сценарий

- Интернет-магазин: процесс заказа включает создание заказа, списание оплаты и резервирование товара.
- Каждый сервис публикует события, на которые реагируют другие сервисы.
- Если что-то идет не так (например, оплата не проходит), запускаются компенсации.
#### Используем каналы как имитацию шины событий

- В реальной системе это был бы брокер сообщений (Kafka, RabbitMQ), но для примера используем каналы Go.
##### Пример кода с комментариями
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

// Event — структура события для шины
type Event struct {
    Type     string // тип события (OrderCreated, PaymentProcessed, etc.)
    OrderID  int    // идентификатор заказа
    Amount   int    // сумма (для оплаты)
    Quantity int    // количество (для инвентаря)
    Success  bool   // успешность операции
}

// OrderService — сервис заказов
type OrderService struct {
    orders map[int]bool // имитация базы заказов
    bus    chan Event   // шина событий
}

func (s *OrderService) HandleOrder(orderID, amount, quantity int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("OrderService: Создание заказа %d\n", orderID)
    s.orders[orderID] = true // создаем заказ
    // публикуем событие о создании заказа
    s.bus <- Event{Type: "OrderCreated", OrderID: orderID, Amount: amount, Quantity: quantity, Success: true}
}

func (s *OrderService) Listen(wg *sync.WaitGroup) {
    defer wg.Done()
    for event := range s.bus { // слушаем события
        if event.Type == "PaymentFailed" && event.OrderID != 0 {
            fmt.Printf("OrderService: Отмена заказа %d из-за неудачной оплаты\n", event.OrderID)
            delete(s.orders, event.OrderID) // компенсируем создание заказа
        }
    }
}

// PaymentService — сервис оплаты
type PaymentService struct {
    payments map[int]int // имитация базы платежей
    bus      chan Event  // шина событий
}

func (s *PaymentService) Listen(wg *sync.WaitGroup) {
    defer wg.Done()
    for event := range s.bus { // слушаем события
        if event.Type == "OrderCreated" {
            fmt.Printf("PaymentService: Обработка платежа для заказа %d на сумму %d\n", event.OrderID, event.Amount)
            if event.Amount <= 0 { // имитация ошибки оплаты
                s.bus <- Event{Type: "PaymentFailed", OrderID: event.OrderID, Success: false}
                continue
            }
            s.payments[event.OrderID] = event.Amount // списываем деньги
            s.bus <- Event{Type: "PaymentProcessed", OrderID: event.OrderID, Success: true}
        } else if event.Type == "InventoryFailed" && event.OrderID != 0 {
            fmt.Printf("PaymentService: Возврат платежа для заказа %d\n", event.OrderID)
            delete(s.payments, event.OrderID) // компенсируем платеж
        }
    }
}

// InventoryService — сервис инвентаря
type InventoryService struct {
    stock map[int]int // имитация склада
    bus   chan Event  // шина событий
}

func (s *InventoryService) Listen(wg *sync.WaitGroup) {
    defer wg.Done()
    for event := range s.bus { // слушаем события
        if event.Type == "PaymentProcessed" {
            fmt.Printf("InventoryService: Резервирование %d единиц для заказа %d\n", event.Quantity, event.OrderID)
            if event.Quantity > 5 { // имитация ошибки инвентаря
                s.bus <- Event{Type: "InventoryFailed", OrderID: event.OrderID, Success: false}
                continue
            }
            s.stock[event.OrderID] = event.Quantity // резервируем товар
            s.bus <- Event{Type: "InventoryReserved", OrderID: event.OrderID, Success: true}
        } else if event.Type == "PaymentFailed" && event.OrderID != 0 {
            // ничего не делаем, так как резерв еще не произошел
        }
    }
}

func main() {
    bus := make(chan Event, 10) // шина событий (буферизированный канал)
    var wg sync.WaitGroup       // для ожидания завершения сервисов

    // инициализируем сервисы
    orderSvc := OrderService{orders: make(map[int]bool), bus: bus}
    paymentSvc := PaymentService{payments: make(map[int]int), bus: bus}
    invSvc := InventoryService{stock: make(map[int]int), bus: bus}

    // запускаем слушателей для каждого сервиса
    wg.Add(3)
    go orderSvc.Listen(&wg)
    go paymentSvc.Listen(&wg)
    go invSvc.Listen(&wg)

    // тест 1: успешная сага
    fmt.Println("Тест 1: Успешная сага")
    wg.Add(1)
    go orderSvc.HandleOrder(1, 100, 2, &wg)
    time.Sleep(500 * time.Millisecond) // даем время на обработку

    // тест 2: ошибка оплаты
    fmt.Println("\nТест 2: Ошибка оплаты")
    wg.Add(1)
    go orderSvc.HandleOrder(2, -10, 3, &wg)
    time.Sleep(500 * time.Millisecond)

    // тест 3: ошибка инвентаря
    fmt.Println("\nТест 3: Ошибка инвентаря")
    wg.Add(1)
    go orderSvc.HandleOrder(3, 200, 10, &wg)
    time.Sleep(500 * time.Millisecond)

    close(bus) // закрываем шину событий
    wg.Wait()  // ждем завершения всех слушателей
}
```

**Вывод** (примерный):
```text
Тест 1: Успешная сага
OrderService: Создание заказа 1
PaymentService: Обработка платежа для заказа 1 на сумму 100
InventoryService: Резервирование 2 единиц для заказа 1

Тест 2: Ошибка оплаты
OrderService: Создание заказа 2
PaymentService: Обработка платежа для заказа 2 на сумму -10
OrderService: Отмена заказа 2 из-за неудачной оплаты

Тест 3: Ошибка инвентаря
OrderService: Создание заказа 3
PaymentService: Обработка платежа для заказа 3 на сумму 200
InventoryService: Резервирование 10 единиц для заказа 3
PaymentService: Возврат платежа для заказа 3
OrderService: Отмена заказа 3 из-за неудачной оплаты
```

---
### Объяснение кода

#### Компоненты

1. **Event**: Структура события с типом и данными (`OrderID`, `Amount`, `Quantity`, `Success`).
2. **OrderService**:
    - Создает заказ и публикует `OrderCreated`.
    - Слушает `PaymentFailed` для компенсации.
3. **PaymentService**:
    - Реагирует на `OrderCreated`, выполняет платеж и публикует `PaymentProcessed` или `PaymentFailed`.
    - Слушает `InventoryFailed` для возврата платежа.
4. **InventoryService**:
    - Реагирует на `PaymentProcessed`, резервирует товар и публикует `InventoryReserved` или `InventoryFailed`.
#### Логика хореографии

- **События как триггеры**: Каждый сервис слушает шину событий (`bus`) и реагирует на нужные типы событий.
- **Компенсация**:
    - Если `PaymentFailed`, `OrderService` отменяет заказ.
    - Если `InventoryFailed`, `PaymentService` возвращает деньги, а `OrderService` отменяет заказ.
- **Асинхронность**: Сервисы работают независимо, обмениваясь событиями через канал.
#### Тонкости

- **Событийная шина**: Здесь `bus` — простой канал, в реальности это брокер сообщений с гарантией доставки.
- **Проверка ошибок**: Условия вроде `amount <= 0` или `quantity > 5` имитируют сбои.
- **Завершение**: Закрытие `bus` завершает работу всех слушателей.

---
### Преимущества хореографии

- **Децентрализация**: Нет единой точки управления, каждый сервис автономен.
- **Гибкость**: Легко добавить новый сервис, подписав его на события.
- **Асинхронность**: Сервисы работают независимо, не блокируя друг друга.
### Недостатки

- **Сложность отладки**: Труднее отследить поток событий и ошибки.
- **Нет центрального состояния**: Нужно логировать события для анализа.
- **Зависимость от шины**: Если события теряются, сага может сломаться.

---
### Коротко

- **Saga**: Разбивает транзакцию на шаги с компенсациями для распределенных систем.
- **Типы**: Хореография (события), Оркестрация (координатор).
- **Хореография**: Сервисы обмениваются событиями через шину, выполняя шаги и компенсации.
- **Пример**: Заказ → Оплата → Резерв, с откатами при сбоях через события.
- **Особенности**: Децентрализовано, гибко, но сложнее контролировать.
- **Тонкости**: Идемпотентность, состояние, асинхронность.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Что такое ACID]]

------
**Links** (Внешние ссылки)
-
