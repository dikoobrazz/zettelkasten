2024120300:02
___
Date: 03-12-2024 | 00:02
Tags: #go #panic 
mapofcontents: 
___
## Оператор panic

> Оператор **panic** позволяет сгенерировать [[00 Go - Обработка ошибок|ошибку]] и выйти из программы:

```Go
package main
import "fmt"
 
func main() {
    fmt.Println(divide(15, 5))
    fmt.Println(divide(4, 0))
    fmt.Println("Program has been finished")
}
func divide(x, y float64) float64{
    if y == 0{ 
        panic("division by zero!")
    }
    return x / y
}

// 3
// panic: division by zero!
```

> [!info] INFO
> Оператору panic мы можем передать любое сообщение, которое будет выводиться на консоль. Например, в данном случае в функции divide, если второй параметр равен 0, то осуществляется вызов `panic("division by zero!")`.



-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Go - Обработка ошибок]]

------
**Links** (Внешние ссылки)
-
