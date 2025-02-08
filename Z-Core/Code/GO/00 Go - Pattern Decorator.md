2025020415:42
___
Date: 04-02-2025 | 15:42
Tags: #go #pattern #decorator
mapofcontents:
___
## Pattern Decorator (Декоратор)

### Когда использовать?

Когда нужно **добавлять новую функциональность объекту без изменения его кода**.
В Go **нет классов и наследования**, поэтому вместо `extends` (как в Java) мы используем **функции-обертки**.

### Применение в Go:

- Добавление логирования, кэширования, проверки прав доступа.

**Пример Decorator в Go**
```go
package main

import (
	"fmt"
	"strings"
)

// Базовый интерфейс
type Notifier interface {
	Send(message string)
}

// Конкретный объект
type EmailNotifier struct{}

func (e EmailNotifier) Send(message string) {
	fmt.Println("Email:", message)
}

// Декоратор: логирование
type LoggerDecorator struct {
	wrapped Notifier
}

func (d LoggerDecorator) Send(message string) {
	fmt.Println("[LOG] Отправка сообщения...")
	d.wrapped.Send(message)
}

// Декоратор: шифрование
type EncryptDecorator struct {
	wrapped Notifier
}

func (d EncryptDecorator) Send(message string) {
	encrypted := strings.ToUpper(message) // Простое "шифрование"
	fmt.Println("[ENCRYPTED] Отправка:", encrypted)
	d.wrapped.Send(encrypted)
}

func main() {
	email := EmailNotifier{}

	// Добавляем логирование
	loggedEmail := LoggerDecorator{wrapped: email}
	loggedEmail.Send("Привет!") 

	// Добавляем шифрование и логирование
	secureLoggedEmail := EncryptDecorator{wrapped: loggedEmail}
	secureLoggedEmail.Send("Секретное сообщение")
}
```

**Отличие от других языков:**
- В Go **нет наследования**, поэтому вместо классов-декораторов используются **структуры с полем wrapped**.
- Можно **гибко комбинировать декораторы** (`EncryptDecorator(LoggerDecorator(email))`).

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
-
