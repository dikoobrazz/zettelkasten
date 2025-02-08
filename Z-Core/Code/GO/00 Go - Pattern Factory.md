2025020403:01
___
Date: 04-02-2025 | 03:01
Tags: #go #pattern #factory
mapofcontents: 
___
## Factory Pattern (Фабричный метод)

### Когда использовать?

Когда нужно **скрыть сложную логику создания объектов** и предоставить единый интерфейс для их получения.

**Пример Factory в Go**
```go
package main

import "fmt"

// Интерфейс продукта
type Animal interface {
	Speak() string
}

// Конкретные реализации
type Dog struct{}
func (d Dog) Speak() string { return "Гав!" }

type Cat struct{}
func (c Cat) Speak() string { return "Мяу!" }

// Фабрика
func AnimalFactory(animalType string) Animal {
	switch animalType {
	case "dog":
		return Dog{}
	case "cat":
		return Cat{}
	default:
		return nil
	}
}

func main() {
	dog := AnimalFactory("dog")
	fmt.Println(dog.Speak()) // Гав!

	cat := AnimalFactory("cat")
	fmt.Println(cat.Speak()) // Мяу!
}
```

**Отличие от других языков:**
- **Нет классов** → вместо них **фабричная функция**.
- Вместо `new Animal()` (как в Java) → просто вызываем `AnimalFactory("dog")`.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
-
