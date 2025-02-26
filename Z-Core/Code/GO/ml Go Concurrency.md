2025021400:46
___
Date: 14-02-2025 | 00:46
Tags: #go #concurrency 
mapofcontents: 
___
## Concurrency in Go

### Синхронизация

| Классический подход | CSP подход |
| ------------------- | ---------- |
| мьютексы            | каналы     |
| wait group          | селект     |
> Don't communicate by sharing memory; 
> Share memory by communicating.

> **Go рекомендует CSP подход. Но по факту используются оба.**

### Классический подход
- **[[00 Go - WaitGroup|WaitGroup]]**
- **[[00 Go - Mutex|Mutex]]**
- **[[00 Go - RWMutex|RWMutex]]**
- Cond
- **[[00 Go - Once|Once]]**
- Pool
- sync/atomic

### CSP подход
- **[[00 Go - Channel|Chanel (Каналы)]]**

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Go - WaitGroup]]
- [[00 Go - Mutex]]
- [[00 Go - RWMutex]]
- [[00 Go - Once]]

- [[00 Go - Channel]]

------
**Links** (Внешние ссылки)
-
