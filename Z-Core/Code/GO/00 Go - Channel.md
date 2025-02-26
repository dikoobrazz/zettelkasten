2025021402:52
___
Date: 14-02-2025 | 02:52
Tags: #go #concurrency #channel 
mapofcontents:
___
## Channel (Каналы)

> Чтение и запись в канал - это конкурентно-безопасные операции

### Небуферезированный канал

**Example**
```go
var ch chan int // nil - не делать так

ch := make(chan int)

var rch<- chan int = ch // Только чтение 
var wch chan<- int = ch // Только запись

```

### Буферезированный канал

**Exqample**
```go
ch := make(chan int, 1) // 1 - размер буфера

_ = len(ch) // 0
_ = cap(ch) // 1

ch<- 5

_ = len(ch) // 1
_ = cap(ch) // 1
```

### Закрытие канала

> Закрытие канала - это также способ отправить сообщение сразу нескольким горутинам

**Example**
```go
ch := make(chan int, 3)
ch<- 2
ch<- 4

close(ch)

_ = <-ch // 2
_ = <-ch // 4
_ = <-ch // 0
_ = <-ch // 0

close(ch) // Паника
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
