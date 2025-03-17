2025030422:07
___
Date: 04-03-2025 | 22:07
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## На каком протоколе работает gRPC

> **gRPC** работает на протоколе **HTTP/2**

---
### Почему HTTP/2

- **gRPC** (Google Remote Procedure Call) — это высокопроизводительный фреймворк для удаленного вызова процедур, разработанный Google.
- HTTP/2 выбран как транспортный протокол, потому что он:
    1. **Мультиплексирование**: Позволяет отправлять несколько запросов и ответов одновременно по одному соединению, избегая "head-of-line blocking" (блокировки в очереди), как в HTTP/1.1.
    2. **Двоичный формат**: Использует компактные двоичные фреймы вместо текстовых, что уменьшает накладные расходы.
    3. **Сжатие заголовков**: Снижает объем передаваемых метаданных с помощью HPACK.
    4. **Двусторонняя связь**: Поддерживает стримы (потоки), что идеально для gRPC (уни- и бинаправленные стримы).

---
### Как это работает?

1. **Транспорт**: HTTP/2 выступает как нижний уровень, обеспечивая соединение между клиентом и сервером.
2. **Сериализация**: gRPC использует **Protocol Buffers** (Protobuf) для кодирования данных в бинарный формат, который передается в теле HTTP/2-запросов.
3. **Метаданные**: Заголовки HTTP/2 используются для передачи метаданных (например, аутентификация), а тело — для Payload (данных).

#### Пример HTTP/2 в gRPC

- Клиент отправляет запрос (например, `POST /service/method`) с заголовками HTTP/2:
    - `:method: POST`
    - `:path: /service/method`
    - `content-type: application/grpc`
- Тело запроса — сериализованный Protobuf.
- Сервер отвечает в том же соединении, используя стримы.

---
##### Пример в Go
```go
package main

import (
    "context"
    "log"
    "net"

    "google.golang.org/grpc"
    pb "google.golang.org/grpc/examples/helloworld/helloworld"
)

// серверный код gRPC
type server struct {
    pb.UnimplementedGreeterServer
}

func (s *server) SayHello(ctx context.Context, req *pb.HelloRequest) (*pb.HelloReply, error) {
    return &pb.HelloReply{Message: "Hello, " + req.Name}, nil
}

func main() {
    lis, err := net.Listen("tcp", ":50051") // слушаем TCP порт
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }
    s := grpc.NewServer() // создаем gRPC сервер поверх HTTP/2
    pb.RegisterGreeterServer(s, &server{})
    log.Printf("server listening at %v", lis.Addr())
    if err := s.Serve(lis); err != nil {
        log.Fatalf("failed to serve: %v", err)
    }
}
```

**Комментарий**:

- `grpc.NewServer()` создает сервер, работающий на HTTP/2.
- Запросы и ответы передаются через HTTP/2 стримы.

---
### Тонкости

- **HTTP/2 обязательна**: gRPC не работает на HTTP/1.1 без прокси (например, `grpc-web` адаптирует его).
- **TLS**: По умолчанию gRPC использует HTTPS (HTTP/2 с шифрованием), но можно отключить для тестов.
- **Protobuf**: Данные кодируются в Protobuf, что делает их компактнее и быстрее, чем JSON.

---
### Коротко

- **Протокол**: gRPC работает на **HTTP/2**.
- **Почему**: Мультиплексирование, двоичный формат, стримы.
- **Как**: HTTP/2 как транспорт, Protobuf как сериализация.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
