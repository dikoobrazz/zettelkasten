2025021402:41
___
Date: 14-02-2025 | 02:41
Tags: #go #concurrency #once
mapofcontents:
___
## Once

> **Once** - гарантирует, что функция отправленная в **Once** будет выполнена только один раз.
> Даже при наличии нескольких потоков.

**Example**
```go
var once sync.Once

for i := 0; i < 10; i++ {
	go func() {
		once.Do(initConn)
	}()
}
```


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Concurrency]]

------
**Links** (Внешние ссылки)
-
