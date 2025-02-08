2025020406:01
___
Date: 04-02-2025 | 06:01
Tags: #go #pattern #observer
mapofcontents: [[zero-links|OO Links]]
___
## Pattern Observer (Наблюдатель)

### Когда использовать?

Когда нужно **уведомлять несколько подписчиков** о каком-то событии (например, система уведомлений).

### Применение в Go:

- Реализация событийно-ориентированных систем.
- Уведомления о изменениях состояния.

**Пример Observer в Go**
```go
package main

import "fmt"

// Интерфейс наблюдателя
type Observer interface {
	Update(message string)
}

// Конкретные наблюдатели
type User struct {
	Name string
}

func (u *User) Update(message string) {
	fmt.Printf("%s получил уведомление: %s\n", u.Name, message)
}

// Субъект (издатель)
type NewsPublisher struct {
	observers []Observer
}

func (n *NewsPublisher) Subscribe(o Observer) {
	n.observers = append(n.observers, o)
}

func (n *NewsPublisher) NotifyAll(message string) {
	for _, o := range n.observers {
		o.Update(message)
	}
}

func main() {
	news := &NewsPublisher{}

	user1 := &User{Name: "Андрей"}
	user2 := &User{Name: "Мария"}

	news.Subscribe(user1)
	news.Subscribe(user2)

	news.NotifyAll("Новая статья про Go!") 
	// Андрей получил уведомление: Новая статья про Go!
	// Мария получила уведомление: Новая статья про Go!
}
```

**Отличие от других языков:**
- **Подписчики — это структуры**, а не классы.
- **Используется слайс observers** вместо списков объектов.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
-
