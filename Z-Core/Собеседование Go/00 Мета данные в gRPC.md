2025030621:43
___
Date: 06-03-2025 | 21:43
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Мета данные в gRPC

Метаданные в gRPC — это дополнительные данные, которые передаются через HTTP/2 заголовки вместе с запросами и ответами, помимо основного тела сообщения в Protobuf.
### Назначение

Они используются для передачи информации, не связанной с полезной нагрузкой, например, токенов аутентификации, идентификаторов запросов или настроек.
### Реализация

В Go метаданные обрабатываются через пакет google.golang.org/grpc/metadata. Их можно задавать как пары ключ-значение, например, `auth-token: 123`, и они доступны в `context.Context` на клиенте и сервере.

##### Пример кода с комментариями
```go
package main

import (
    "context"
    "fmt"
    "log"
    "net"

    "google.golang.org/grpc"
    "google.golang.org/grpc/metadata"
    pb "google.golang.org/grpc/examples/helloworld/helloworld"
)

type server struct {
    pb.UnimplementedGreeterServer
}

// SayHello — обработка запроса с чтением метаданных
func (s *server) SayHello(ctx context.Context, req *pb.HelloRequest) (*pb.HelloReply, error) {
    // извлекаем метаданные из контекста
    md, ok := metadata.FromIncomingContext(ctx)
    if !ok {
        return nil, fmt.Errorf("нет метаданных")
    }
    // читаем значение по ключу "auth-token"
    if token := md.Get("auth-token"); len(token) > 0 {
        fmt.Println("Получен токен:", token[0])
    }
    return &pb.HelloReply{Message: "Hello, " + req.Name}, nil
}

func main() {
    lis, err := net.Listen("tcp", ":50051") // слушаем порт
    if err != nil {
        log.Fatalf("ошибка: %v", err)
    }
    s := grpc.NewServer() // создаем сервер
    pb.RegisterGreeterServer(s, &server{})

    // клиентская часть: отправка запроса с метаданными
    go func() {
        conn, err := grpc.Dial("localhost:50051", grpc.WithInsecure())
        if err != nil {
            log.Fatalf("ошибка: %v", err)
        }
        defer conn.Close()
        client := pb.NewGreeterClient(conn)

        // создаем контекст с метаданными
        ctx := metadata.AppendToOutgoingContext(context.Background(), "auth-token", "123")
        resp, err := client.SayHello(ctx, &pb.HelloRequest{Name: "Alice"})
        if err != nil {
            log.Fatalf("ошибка вызова: %v", err)
        }
        fmt.Println("Ответ:", resp.Message) // Ответ: Hello, Alice
    }()

    log.Println("Сервер запущен")
    s.Serve(lis)
}
```

- **Комментарий**:
    - Клиент добавляет метаданные (`auth-token`) через `metadata.AppendToOutgoingContext`.
    - Сервер извлекает их с помощью `metadata.FromIncomingContext` и читает по ключу.

### Особенности

Метаданные передаются отдельно от Protobuf, что позволяет добавлять функциональность без изменения контракта `.proto`.

### Итог

Метаданные в gRPC — это гибкий способ передачи служебной информации через HTTP/2 заголовки, расширяющий возможности взаимодействия между клиентом и сервером.



-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
