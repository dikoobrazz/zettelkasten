2025030622:30
___
Date: 06-03-2025 | 22:30
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Graceful shutdown

> **Graceful shutdown** (грациозное завершение) в Go — это процесс, при котором сервер завершает свою работу плавно, позволяя обработать текущие запросы, не принимая новые, перед полной остановкой. Это важно для веб-приложений, чтобы избежать прерывания активных соединений и сохранить данные. В Go это реализуется с использованием пакета http.Server и механизма сигналов ОС.

---
##### Пример кода с graceful shutdown
```go
package main

import (
	"context"
	"fmt"
	"log"
	"net/http"
	"os"
	"os/signal"
	"time"
)

func main() {
	// Создаем HTTP-сервер с базовым обработчиком
	srv := &http.Server{
		Addr:    ":8080",           // Адрес и порт сервера
		Handler: http.HandlerFunc(handler), // Обработчик запросов
	}

	// Запускаем сервер в отдельной горутине
	go func() {
		log.Printf("Starting server on %s", srv.Addr)
		if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
			// Ошибка, если сервер завершился не из-за Shutdown
			log.Fatalf("Server failed: %v", err)
		}
	}()

	// Создаем канал для получения сигналов ОС (например, Ctrl+C)
	sigChan := make(chan os.Signal, 1)
	// Регистрируем сигналы SIGINT (Ctrl+C) и SIGTERM
	signal.Notify(sigChan, os.Interrupt, os.Kill)

	// Ожидаем сигнал завершения
	<-sigChan
	log.Println("Received shutdown signal, initiating graceful shutdown...")

	// Создаем контекст с таймаутом для завершения работы
	ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
	defer cancel()

	// Выполняем graceful shutdown сервера
	if err := srv.Shutdown(ctx); err != nil {
		log.Printf("Shutdown failed: %v", err)
	} else {
		log.Println("Server gracefully stopped")
	}
}

// Простой обработчик запросов
func handler(w http.ResponseWriter, r *http.Request) {
	// Симулируем долгую задачу (например, обработку запроса)
	time.Sleep(2 * time.Second)
	fmt.Fprintf(w, "Hello, this is a response!\n")
}
```

### Комментарии к коду

1. **Создание сервера**:
    - Используем `http.Server` с указанием порта (`:8080`) и обработчика (`handler`).
    - `HandlerFunc` — это простая функция, которая отвечает на запросы.
2. **Запуск в горутине**:
    - `srv.ListenAndServe()` запускается в отдельной горутине, чтобы основной поток мог ждать сигналов завершения.
    - Проверяем ошибку: если она не `http.ErrServerClosed` (нормальное завершение), это сбой.
3. **Обработка сигналов**:
    - `sigChan` — канал для сигналов ОС.
    - `signal.Notify` подписывает канал на сигналы `SIGINT` (Ctrl+C) и `SIGTERM` (от kill).
    - `<-sigChan` блокирует выполнение до получения сигнала.
4. **Graceful Shutdown**:
    - После получения сигнала создаем контекст с таймаутом (5 секунд), чтобы дать серверу время завершить текущие запросы.
    - `srv.Shutdown(ctx)` останавливает сервер: новые запросы отклоняются, а текущие завершаются.
    - Если таймаут истекает, незавершенные запросы прерываются.
5. **Обработчик**:
    - `handler` имитирует долгую задачу с `time.Sleep`. В реальном приложении это может быть запрос к базе данных или внешнему API.

### Как это работает

1. Запустите программу: `go run main.go`.
2. Сервер начнет слушать на `:8080`.
3. Отправьте запрос, например, через curl http://localhost:8080.
4. Нажмите `Ctrl+C` в терминале.
5. Если запрос был в процессе, он завершится (до 5 секунд), после чего сервер остановится.

##### Пример вывода
```text
2025/03/06 12:00:00 Starting server on :8080
<curl запрос>
2025/03/06 12:00:05 Received shutdown signal, initiating graceful shutdown...
2025/03/06 12:00:07 Server gracefully stopped
```

### Дополнительные заметки

- **Таймаут**: 5 секунд — это пример. В реальном проекте подберите значение в зависимости от максимального времени обработки запросов.
- **Логи**: Используйте более продвинутый логгер (например, `logrus`) для реальных приложений.
- **Дополнительные сервисы**: Если у вас есть другие ресурсы (база данных, кэш), их тоже нужно закрывать после `Shutdown`.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
