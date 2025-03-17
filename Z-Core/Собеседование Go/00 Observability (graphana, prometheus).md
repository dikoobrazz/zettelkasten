2025030716:03
___
Date: 07-03-2025 | 16:03
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Что использовал для observability? (graphana, prometheus)

> Для обеспечения **observability** (наблюдаемости) в распределенных системах, таких как микросервисы, используются инструменты, которые помогают собирать, хранить, анализировать и визуализировать метрики, логи и трассировки. **Prometheus** и **Grafana** — это популярный стек, который часто применяется вместе, но они решают разные задачи.

---
### Что такое Observability?

> **Observability** — это способность понимать состояние системы на основе внешних данных:

1. **Метрики**: Количественные показатели (например, latency, error rate).
2. **Логи**: Текстовые записи событий.
3. **Трассировки**: Следы запросов через сервисы.

Prometheus и Grafana покрывают метрики и визуализацию, но для полной наблюдаемости нужны дополнительные инструменты.

---
### Prometheus

> **Что это**: Prometheus — это система мониторинга и хранения временных рядов (time-series database), которая собирает метрики с приложений через pull-модель (запрашивает данные по HTTP).

**Зачем нужен**:

- Сбор метрик (CPU, память, запросы/с, ошибки).
- Хранение данных с временной меткой.
- Мощный язык запросов (PromQL).

**Как работает**:

- Приложение предоставляет endpoint (например, `/metrics`) с метриками в формате Prometheus.
- Prometheus периодически "скрейпит" (scrapes) эти данные.

**Пример в Go**:
```go
package main

import (
    "net/http"
    "github.com/prometheus/client_golang/prometheus"
    "github.com/prometheus/client_golang/prometheus/promhttp"
)

var (
    requestsTotal = prometheus.NewCounter(
        prometheus.CounterOpts{
            Name: "http_requests_total",
            Help: "Total number of HTTP requests",
        },
    )
    requestDuration = prometheus.NewHistogram(
        prometheus.HistogramOpts{
            Name:    "http_request_duration_seconds",
            Help:    "Duration of HTTP requests",
            Buckets: prometheus.LinearBuckets(0.01, 0.05, 10),
        },
    )
)

func init() {
    prometheus.MustRegister(requestsTotal)
    prometheus.MustRegister(requestDuration)
}

func handler(w http.ResponseWriter, r *http.Request) {
    start := time.Now()
    requestsTotal.Inc()
    time.Sleep(50 * time.Millisecond) // Симуляция работы
    requestDuration.Observe(time.Since(start).Seconds())
    w.Write([]byte("Hello"))
}

func main() {
    http.HandleFunc("/api", handler)
    http.Handle("/metrics", promhttp.Handler()) // Endpoint для Prometheus
    http.ListenAndServe(":8080", nil)
}
```

- **Логика**: Счетчик `requestsTotal` считает запросы, гистограмма `requestDuration` измеряет время обработки.

**Конфигурация Prometheus** (`prometheus.yml`):
```yml
scrape_configs:
  - job_name: 'my-service'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:8080']
```

- Запуск: prometheus `--config.file=prometheus.yml`.

**Плюсы**:

- Легкая интеграция с Go (`prometheus/client_golang`).
- Высокая производительность.
- Гибкие запросы (PromQL).

**Минусы**:

- Не для логов или трассировок.
- Короткосрочное хранение (обычно 15 дней).

---
### Grafana

> **Что это**: Grafana — это инструмент визуализации, который подключается к источникам данных (например, Prometheus) и строит дашборды с графиками, таблицами и алертами.

**Зачем нужен**:

- Визуализация метрик (latency, throughput, ошибки).
- Настройка алертов (например, "если ошибка > 5%").
- Удобный UI для анализа.

**Как работает**:

- Подключается к Prometheus как datasource.
- Использует PromQL для запросов и отображения данных.

**Пример настройки**:

1. Установите Grafana: `docker run -d -p 3000:3000 grafana/grafana`.
2. Зайдите на http://localhost:3000 (логин: admin, пароль: admin).
3. Добавьте Prometheus как источник данных:
    - URL: http://localhost:9090 (где работает Prometheus).
4. Создайте дашборд:
    - Метрика: `rate(http_requests_total[5m]`) — запросы/с.
    - Метрика: `histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))` — 95-й перцентиль задержки.

**Плюсы**:

- Интуитивный интерфейс.
- Поддержка множества источников (Prometheus, Loki, Jaeger).
- Алерты через email, Slack и т.д.

**Минусы**:

- Только визуализация, не собирает данные сам.

---
### Полная Observability: Дополнение к Prometheus и Grafana

Prometheus и Grafana покрывают метрики, но для логов и трассировок нужны другие инструменты.

#### 1. Loki (Логи)

- **Что это**: Легковесная система для хранения и поиска логов, интегрируется с Grafana.
- **Зачем**: Собирает логи от микросервисов.
- **Пример в Go**:
```go
import "github.com/grafana/loki/logproto"

// Используйте logrus с Loki hook
import "github.com/sirupsen/logrus"
lokiHook := loki.NewLokiHook("http://localhost:3100/loki/api/v1/push")
logrus.AddHook(lokiHook)
logrus.Info("Service started")
```

- **Конфигурация**: Запустите Loki (`docker run -d grafana/loki`).

#### 2. Jaeger или OpenTelemetry (Трассировки)

- **Что это**: Системы для distributed tracing, отслеживают путь запроса через сервисы.
- **Зачем**: Диагностика задержек и ошибок между микросервисами.
- **Пример в Go**:
```go
import "go.opentelemetry.io/otel"

tracer := otel.Tracer("my-service")
ctx, span := tracer.Start(context.Background(), "handle-request")
defer span.End()
```

- **Конфигурация**: Экспорт трассировок в Jaeger (`docker run -d -p 16686:16686` `jaegertracing/all-in-one`).

#### 3. ELK Stack (Elasticsearch, Logstash, Kibana)

- **Что это**: Альтернатива для логов (более тяжелая, чем Loki).
- **Зачем**: Анализ логов и поиск.

---
### Как использовать Prometheus и Grafana вместе?

1. **Собирайте метрики с Prometheus**:
    - Инструментируйте код в Go (`prometheus/client_golang`).
    - Настройте скрейпинг в `prometheus.yml`.
2. **Визуализируйте в Grafana**:
    - Подключите Prometheus как источник.
    - Создайте дашборды (например, "HTTP Requests Rate", "Latency").
3. **Добавьте алерты**:
    - В Grafana настройте уведомления (если `rate(errors_total[5m]) > 0.01`).

**Пример дашборда**:

- График: `rate(http_requests_total[5m]`).
- Панель: Среднее время обработки (`avg(rate(http_request_duration_seconds[5m]))`).

---
### Практические советы для Go

1. **Метрики**:
    - Используйте стандартные типы: Counter, Gauge, Histogram.
    - Добавьте labels для фильтрации:
```go
requestsTotal = prometheus.NewCounterVec(
    prometheus.CounterOpts{Name: "http_requests_total"},
    []string{"method", "endpoint"},
)
requestsTotal.WithLabelValues("GET", "/api").Inc()
```

2. **Логи**:
    - Интегрируйте logrus с Loki для простоты.
3. **Трассировки**:
    - Используйте OpenTelemetry для унификации (экспорт в Jaeger или OTLP).
4. **Развертывание**:
    - В Kubernetes используйте Helm-чарты:
```bash
helm install prometheus prometheus-community/prometheus
helm install grafana grafana/grafana
```

---
### Что выбрать?

- **Prometheus**: Для метрик (обязательно).
- **Grafana**: Для визуализации и алертов (обязательно с Prometheus).
- **Loki**: Для логов (легкий и совместим с Grafana).
- **Jaeger/OpenTelemetry**: Для трассировок (если много микросервисов).
- **ELK**: Если нужен мощный анализ логов (альтернатива Loki).

**Рекомендуемый стек**:

- Prometheus + Grafana + Loki + OpenTelemetry — покрывает метрики, логи и трассировки с минимальной сложностью.

---
### Итог

Prometheus собирает метрики, Grafana визуализирует их. Для полной наблюдаемости добавьте Loki для логов и Jaeger/OpenTelemetry для трассировок. В Go это легко интегрируется через библиотеки (`prometheus/client_golang`, `opentelemetry`).

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
