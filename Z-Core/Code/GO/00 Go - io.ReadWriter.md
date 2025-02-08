2025012922:16
___
Date: 29-01-2025 | 22:16
Tags: #go #io #readwriter
mapofcontents: 
___
## io.ReadWriter

>**`io.ReadWriter`**
 Это объединение интерфейсов **[[00 Go - io.Reader|io.Reader]]** и **[[00 Go - io.Writer|io.Writer]]**, которое позволяет работать как с чтением, так и с записью.

```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	// Создаем строку для чтения и записи
	rw := io.ReadWriter(strings.NewReader("Hello, ReadWriter!"))

	// Чтение данных
	buf := make([]byte, 5)
	n, err := rw.Read(buf)
	if err != nil && err != io.EOF {
		fmt.Println("Error:", err)
	}
	fmt.Printf("Read %d bytes: %s\n", n, string(buf[:n]))

	// Запись данных
	_, err = rw.Write([]byte(" - New Data"))
	if err != nil {
		fmt.Println("Error writing:", err)
	}
}
```

**Как это работает:**
- Мы создаем строку, которая поддерживает как чтение, так и запись. Это полезно, если нужно одновременно и читать, и записывать данные в одном потоке.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go IO]]
- [[00 Go - io.Reader]]
- [[00 Go - io.Writer]]
------
**Links** (Внешние ссылки)
-
