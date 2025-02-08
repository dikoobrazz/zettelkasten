2025012922:37
___
Date: 29-01-2025 | 22:37
Tags: #go #io #pipe
mapofcontents:
___
## io.Pipe

>`io.Pipe` позволяет создать пару связанных `io.Reader` и `io.Writer`. Это полезно, если нужно передавать данные между двумя частями программы через канал.

**Пример использования `io.Pipe`**:
```go
package main

import (
	"fmt"
	"io"
)

func main() {
	// Создаем pipe
	pr, pw := io.Pipe()

	// Горутин, который будет записывать в pipe
	go func() {
		defer pw.Close()
		pw.Write([]byte("Hello, Pipe!"))
	}()

	// Читаем данные из pipe
	buf := make([]byte, 1024)
	n, err := pr.Read(buf)
	if err != nil && err != io.EOF {
		fmt.Println("Error:", err)
	}
	fmt.Printf("Read %d bytes: %s\n", n, string(buf[:n]))
}
```

**Как это работает:**
- В этом примере создаются связанные **[[00 Go - io.Reader|Reader]]** и **[[00 Go - io.Writer|Writer]]** через `io.Pipe`. Один горутин записывает данные в pipe, а другой — читает их.
- Это идеально подходит для асинхронной передачи данных между горутинами или потоками

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go IO]]
- [[00 Go - io.Reader]]
- [[00 Go - io.Writer]]

------
**Links** (Внешние ссылки)
-
