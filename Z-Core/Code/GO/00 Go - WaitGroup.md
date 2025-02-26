2025021402:33
___
Date: 14-02-2025 | 02:33
Tags: #go #concurrency #waitgroup #wait
mapofcontents: 
___
## WaitGroup

> **WaitGroup** - очень полезная конструкция, когда нужно запустить несколько gorutines
> **WaitGroup** - работает как счётчик
> **Wait** - блокирует пока пока **WaitGroup** не станет равен нулю

**Example**
```go
var wg sync.WaitGroup

wg.Add(3)

for i := 0; i < 3; i++ {
	go func(){
		defer wg.Done()
		compute()
	}()
}

wg.Wait()
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Concurrency]]

------
**Links** (Внешние ссылки)
-
