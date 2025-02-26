2025021401:29
___
Date: 14-02-2025 | 01:29
Tags: #go #concurrency #mutex #rwmutex
mapofcontents: [[zero-links|OO Links]]
___
## RWMutex

### Несколько читателей один писатель

**Example**
```go
type Service struct {
	mu sync.RWMutex
	m map[string]int
}

func (s *Service) Read(key string) int {
	s.mu.RLock()
	defer s.mu.RUnlock()
	return s.m[key]
}

func (s *Service) Update(key string, val int) {
	s.mu.Lock()
	defer s.mu.Unlock()
	s.m[key] = val
}
```

> RWMutex быстрее Mutex толдько при большом кол-ве читателей.
> Но всё равно стоит использовать для выражения намерения.



-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Concurrency]]

------
**Links** (Внешние ссылки)
-
