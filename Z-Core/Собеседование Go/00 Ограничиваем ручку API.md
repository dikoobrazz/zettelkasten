2025030712:04
___
Date: 07-03-2025 | 12:04
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Ограничиваем ручку API, 10000 в мин

> Ограничение количества запросов к API (**rate limiting**) — это распространенная задача для защиты микросервисов от перегрузки, злоупотребления или DDoS-атак. Чтобы ограничить ручку API до 10 тысяч запросов в минуту, можно использовать несколько подходов.

---
### Что такое Rate Limiting?

Rate Limiting ограничивает число запросов, которые клиент (например, IP-адрес или токен) может отправить за определенный период. В данном случае: **10 000 запросов в минуту**.

**Зачем это нужно**:

- Предотвращение перегрузки сервера.
- Защита от злоупотребления API.
- Обеспечение ресурсов между клиентами.

---
### Подходы к реализации

#### 1. Token Bucket (Токеновое ведро)

**Описание**: Алгоритм, где есть "ведро" с токенами (10 000). Каждый запрос забирает токен. Ведро пополняется с фиксированной скоростью (например, 10 000 токенов в минуту). Если токенов нет, запрос отклоняется.

**Пример в Go** (с библиотекой golang.org/x/time/rate):
```go
package main

import (
    "net/http"
    "sync"
    "golang.org/x/time/rate"
)

var limiter = rate.NewLimiter(166.67, 10000) // 10 000 в минуту = 166.67 в секунду
var mu sync.Mutex

func limit(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        mu.Lock()
        if !limiter.Allow() {
            mu.Unlock()
            http.Error(w, "Too Many Requests", http.StatusTooManyRequests)
            return
        }
        mu.Unlock()
        next.ServeHTTP(w, r)
    })
}

func handleAPI(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("Success"))
}

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/api", handleAPI)
    http.ListenAndServe(":8080", limit(mux))
}
```

- **Логика**:
    - `rate.Limiter`: 166.67 запросов в секунду (10 000 / 60).
    - `Allow()`: Проверяет, доступен ли токен.
    - `sync.Mutex`: Защищает limiter от конкурентного доступа.

**Плюсы**:

- Простота реализации.
- Поддерживает burst-запросы (до 10 000 сразу).

**Минусы**:

- Глобальное ограничение (не по клиентам).

---
#### 2. Rate Limiting по клиенту (IP или токен)

**Описание**: Ограничение применяется индивидуально для каждого клиента на основе IP-адреса, API-токена или другого идентификатора.

**Пример в Go** (с памятью):
```go
package main

import (
    "net/http"
    "sync"
    "time"
)

type client struct {
    limiter  *rate.Limiter
    lastSeen time.Time
}

var (
    clients = make(map[string]*client)
    mu      sync.Mutex
)

func getLimiter(ip string) *rate.Limiter {
    mu.Lock()
    defer mu.Unlock()

    c, exists := clients[ip]
    if !exists {
        limiter := rate.NewLimiter(166.67, 10000) // 10 000 в минуту
        clients[ip] = &client{limiter: limiter}
        return limiter
    }
    c.lastSeen = time.Now()
    return c.limiter
}

func cleanupClients() {
    for {
        time.Sleep(time.Minute)
        mu.Lock()
        for ip, c := range clients {
            if time.Since(c.lastSeen) > 5*time.Minute {
                delete(clients, ip)
            }
        }
        mu.Unlock()
    }
}

func limit(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        ip := r.RemoteAddr
        limiter := getLimiter(ip)
        if !limiter.Allow() {
            http.Error(w, "Too Many Requests", http.StatusTooManyRequests)
            return
        }
        next.ServeHTTP(w, r)
    })
}

func main() {
    go cleanupClients() // Очистка старых клиентов
    mux := http.NewServeMux()
    mux.HandleFunc("/api", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Success"))
    })
    http.ListenAndServe(":8080", limit(mux))
}
```

- **Логика**:
    - Каждый IP получает свой лимитер.
    - cleanupClients: Удаляет неактивных клиентов, чтобы не переполнять память.

**Плюсы**:

- Ограничение по клиентам.
- Гибкость (можно менять лимит для разных пользователей).

**Минусы**:

- Использование памяти растет с числом клиентов.
- Не масштабируется на несколько инстансов без синхронизации.

---
#### 3. Rate Limiting с Redis

**Описание**: Используйте Redis для хранения счетчиков запросов, что позволяет масштабировать ограничение между несколькими экземплярами сервиса.

**Пример в Go**:
```go
package main

import (
    "net/http"
    "time"
    "github.com/go-redis/redis/v8"
)

var redisClient = redis.NewClient(&redis.Options{
    Addr: "localhost:6379",
})

func limit(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        ip := r.RemoteAddr
        ctx := r.Context()

        key := "rate:" + ip
        count, err := redisClient.Incr(ctx, key).Result()
        if err != nil {
            http.Error(w, "Internal Error", 500)
            return
        }

        if count == 1 {
            redisClient.Expire(ctx, key, time.Minute) // Установить TTL 1 минута
        }
        if count > 10000 {
            http.Error(w, "Too Many Requests", http.StatusTooManyRequests)
            return
        }
        next.ServeHTTP(w, r)
    })
}

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/api", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Success"))
    })
    http.ListenAndServe(":8080", limit(mux))
}
```

- **Логика**:
    - `Incr`: Увеличивает счетчик запросов для IP в Redis.
    - `Expire`: Устанавливает TTL 1 минута, после чего счетчик сбрасывается.
    - Проверка: Если больше 10 000, запрос отклоняется.

**Плюсы**:

- Масштабируемость: Работает с несколькими инстансами.
- Устойчивость: Состояние хранится в Redis.

**Минусы**:

- Зависимость от Redis.
- Дополнительная задержка сети.

---
### Какой подход выбрать?

|**Подход**|**Масштабируемость**|**Сложность**|**Когда использовать**|
|---|---|---|---|
|Token Bucket|Низкая (глобальная)|Низкая|Простое глобальное ограничение|
|По клиенту (память)|Низкая (локальная)|Средняя|Один инстанс, немного клиентов|
|Redis|Высокая|Средняя|Несколько инстансов, высокая нагрузка|

---

### Практические советы для Go

1. **Выбор идентификатора**:
    - IP (`r.RemoteAddr`) подходит для простоты.
    - API-токен (`r.Header.Get("Authorization")`) — для аутентифицированных клиентов.
2. **HTTP-статус**:
    - Возвращайте 429 Too Many Requests с заголовком Retry-After:

```go
w.Header().Set("Retry-After", "60")
http.Error(w, "Too Many Requests", http.StatusTooManyRequests)
```

3. **Логирование**:

- Логируйте превышения для анализа:

```go
log.Printf("Rate limit exceeded for %s", ip)
```

4. **Тестирование**:

- Используйте httptest для проверки:

```go
func TestRateLimit(t *testing.T) {
    for i := 0; i < 10001; i++ {
        req := httptest.NewRequest("GET", "/api", nil)
        w := httptest.NewRecorder()
        limit(http.HandlerFunc(handleAPI)).ServeHTTP(w, req)
        if i > 9999 && w.Code != 429 {
            t.Errorf("Expected 429, got %d", w.Code)
        }
    }
}
```

5. **Масштабирование**:
    - Для нескольких инстансов используйте Redis или внешний API Gateway (например, Kong, Traefik) с встроенным rate limiting.

---
### Итог

Чтобы ограничить ручку API до 10 000 запросов в минуту в Go:

- **Token Bucket**: Простое глобальное ограничение с `rate.Limiter`.
- **По клиенту в памяти**: Для одного инстанса с малым числом клиентов.
- **Redis**: Для масштабируемости и распределенных систем.

Рекомендую начать с Redis, если планируется несколько инстансов, или с rate.Limiter для простоты в одном сервисе.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
