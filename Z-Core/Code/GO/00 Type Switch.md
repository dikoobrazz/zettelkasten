2025012920:36
___
Date: 29-01-2025 | 20:36
Tags: #go #type #switch
mapofcontents: [[zero-links|OO Links]]
___
## Type switch 

Type switch - это расширение обычного switch, которое работает с типами данных.

```go
package main

import "fmt"

func main() {
    var x interface{} = "Hello, Go!"

    switch v := x.(type) {
    case int:
        fmt.Println("Тип int, значение:", v)
    case string:
        fmt.Println("Тип string, значение:", v)
    default:
        fmt.Println("Неизвестный тип")
    }
}
```

Здесь переменная `v` будет иметь тип `string` внутри блока `case string`.

### Работа с множественными типами

`type switch` позволяет проверять типы в блоках `switch`. Это полезно, когда у вас есть переменная типа [[00 Go - Interfaces|`interface{}`]], но вы хотите работать с конкретными типами данных.

```go
package main

import "fmt"

func main() {
    var x interface{} = "Hello, world!"

    switch v := x.(type) {
    case int:
        fmt.Println("Это целое число:", v)
    case string:
        fmt.Println("Это строка:", v)
    default:
        fmt.Println("Неизвестный тип")
    }
}
```

В этом примере мы используем type switch для проверки, какой тип данных находится в переменной `x`. В зависимости от типа будет выполняться соответствующий блок кода.

Когда у нас есть переменная типа интерфейса, но мы не знаем заранее, какой конкретно тип она будет содержать, мы можем использовать **type switch**.

```go
package main

import "fmt"

type Animal interface {
    Speak() string
}

type Dog struct{}
func (d Dog) Speak() string { return "Woof" }

type Cat struct{}
func (c Cat) Speak() string { return "Meow" }

func main() {
    var a Animal = Dog{}
    
    // Type Switch
    switch v := a.(type) {
    case Dog:
        fmt.Println("Это собака:", v.Speak())
    case Cat:
        fmt.Println("Это кошка:", v.Speak())
    default:
        fmt.Println("Неизвестный тип")
    }
}
```

Здесь:
- Мы используем **type switch**, чтобы проверять тип переменной интерфейса `a`.
- Для каждого типа, соответствующего `Dog` или `Cat`, выполняется свой блок кода.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Go - Interfaces]]

------
**Links** (Внешние ссылки)
-
