2025012922:40
___
Date: 29-01-2025 | 22:40
Tags: #go #io #limitreader
mapofcontents: 
___
## io.LimitReader

>`io.LimitReader` ограничивает количество данных, которые можно прочитать из источника, независимо от того, сколько данных реально доступно.

**Пример использования `io.LimitReader`**:
```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	// Источник данных
	src := strings.NewReader("Hello, World!")

	// Ограничиваем чтение до 5 байт
	limitedReader := io.LimitReader(src, 5)

	buf := make([]byte, 8)
	n, err := limitedReader.Read(buf)
	if err != nil && err != io.EOF {
		fmt.Println("Error:", err)
	}
	fmt.Printf("Read %d bytes: %s\n", n, string(buf[:n]))
}
```

**Как это работает:**
- `io.LimitReader` ограничивает количество данных, которые можно прочитать (в данном примере — 5 байт).
- Это полезно для контроля над размером данных, например, когда нужно ограничить размер читаемой части большого файла.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go IO]]

------
**Links** (Внешние ссылки)
-
