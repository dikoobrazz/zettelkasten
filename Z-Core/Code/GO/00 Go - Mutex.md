2025021400:54
___
Date: 14-02-2025 | 00:54
Tags: #go #concurrency #mutex
mapofcontents: 
___
## Mutex in Go

**`var m1 sync.Mutex`** - Правильно
**`mu2 := sync.Mutex{}`** - Неправильно

**Example - Правильно**
```go
type Counter struct {
	mu    sync.Mutex
	count int
}

func (c *Counter) Inc() {
	c.mu.Lock()
	defer c.mu.Unlock()
	c.count++
}
```
**Example - НЕравильно**
```go
type Counter struct {
	mu    sync.Mutex
	count int
}

func (c Counter) Inc() {
	c.mu.Lock() // копия мьютекса
	defer c.mu.Unlock()
	c.count++
}
```

**НЕПРАВИЛЬНО                                   ПРАВИЛЬНО**
![[_resources/mutex_1.png]]

| **НЕПРАВИЛЬНО**                   | **ПРАВИЛЬНО**                 |
| --------------------------------- | ----------------------------- |
| ![[_resources/mutex.png]]         | ![[_resources/mutex_1 1.png]] |
| при панике Unlock не будет вызван |                               |


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
