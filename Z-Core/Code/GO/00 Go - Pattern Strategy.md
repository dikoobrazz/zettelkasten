2025020405:58
___
Date: 04-02-2025 | 05:58
Tags: #go #pattern #strategy
mapofcontents:
___
## Strategy Pattern (Стратегия)

### Когда использовать?

Когда нужно **заменять алгоритмы во время выполнения**, например, выбор способа оплаты (PayPal, кредитка).

### Применение в Go:

- Выбор алгоритмов сортировки, сжатия, шифрования.

**Пример Strategy в Go**
```go
package main

import "fmt"

// Интерфейс стратегии
type PaymentStrategy interface {
	Pay(amount float64)
}

// Конкретные стратегии
type CreditCard struct{}
func (c CreditCard) Pay(amount float64) { fmt.Println("Оплата кредитной картой:", amount) }

type PayPal struct{}
func (p PayPal) Pay(amount float64) { fmt.Println("Оплата через PayPal:", amount) }

// Контекст
type Payment struct {
	strategy PaymentStrategy
}

func (p *Payment) SetStrategy(strategy PaymentStrategy) {
	p.strategy = strategy
}

func (p *Payment) ProcessPayment(amount float64) {
	p.strategy.Pay(amount)
}

func main() {
	payment := &Payment{}

	payment.SetStrategy(CreditCard{})
	payment.ProcessPayment(100.0) // Оплата кредитной картой: 100.0

	payment.SetStrategy(PayPal{})
	payment.ProcessPayment(200.0) // Оплата через PayPal: 200.0
}
```

**Отличие от других языков:**
- **Нет классов**, но есть **интерфейсы + структуры**.
- Стратегии передаются как **структуры с методами**, а не как объекты классов.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
-
