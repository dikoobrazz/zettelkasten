2024120300:07
___
Date: 03-12-2024 | 00:07
Tags: #go #defer
mapofcontents: [[zero-links|OO Links]]
___
## Оператор defer

> [!info] INFO
> Оператор **defer** позволяет выполнить определенную операцию после каких-то действий (даже если сработает **[[00 Go - Оператор panic|panic]]**), при этом не важно, где в реальности вызывается эта функция.

```Go
package main
import "fmt"
 
func main() {
    defer finish()
    fmt.Println("Program has been started")
    fmt.Println("Program is working")
}
 
func finish(){
    fmt.Println("Program has been finished")
}
// Program has been started
// Program is working
// Program has been finished
```

Здесь функция finish вызывается с оператором defer, поэтому данная функция в реальности будет вызываться в самом конце выполнения программы, несмотря на то, что ее вызов определен в начале функции main.

> [!info] INFO
> Если несколько функций вызываются с оператором defer, то те функции, которые вызываются раньше, будут выполняться позже всех

```Go
package main
import  "fmt"
 
func main() {
      
    defer finish()
    defer fmt.Println("Program has been started")
    fmt.Println("Program is working")
}
 
func finish(){
    fmt.Println("Program has been finished")
}

// Program is working
// Program has been started
// Program has been finished
```

> [!info] INFO
> Дополнение: команда _defer_ помещает вызов функции в стек. Поэтому они выполняются в очередности -LIFO (Last-In, First-Out)

>[!warning]
>**defer** запоминает значения переменных, переданных в функцию, на момент объявления defer, а не на момент его вызова.

```Go
a:=5

defer myFunc(a) // когда вызовется myFunc - будет передано значение 5, а не 7

a = 7
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Go - Оператор panic]]

------
**Links** (Внешние ссылки)
-
