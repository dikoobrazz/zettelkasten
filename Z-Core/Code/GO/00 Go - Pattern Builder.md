2025020405:56
___
Date: 04-02-2025 | 05:56
Tags: #go #pattern #builder
mapofcontents: 
___
## Builder Pattern (Строитель)

### Когда использовать?

Когда объект сложный, имеет много параметров, но не все из них обязательны.

**Пример Builder в Go**
```go
package main

import "fmt"

// Структура объекта
type Car struct {
	Brand string
	Color string
	Year  int
}

// Строитель
type CarBuilder struct {
	car Car
}

func NewCarBuilder() *CarBuilder {
	return &CarBuilder{}
}

func (b *CarBuilder) SetBrand(brand string) *CarBuilder {
	b.car.Brand = brand
	return b
}

func (b *CarBuilder) SetColor(color string) *CarBuilder {
	b.car.Color = color
	return b
}

func (b *CarBuilder) SetYear(year int) *CarBuilder {
	b.car.Year = year
	return b
}

func (b *CarBuilder) Build() Car {
	return b.car
}

func main() {
	car := NewCarBuilder().SetBrand("Tesla").SetColor("Red").SetYear(2023).Build()
	fmt.Println(car) // {Tesla Red 2023}
}
```

**Отличие от других языков:**
- В Go **нет сеттеров** → используем **методы с цепочкой вызовов**.
- **Функции возвращают указатель на builder**, чтобы можно было удобно писать `SetBrand().SetColor().Build()`.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
-
