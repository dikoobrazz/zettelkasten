2025012922:33
___
Date: 29-01-2025 | 22:33
Tags: #go #io #multireader
mapofcontents: 
___
## io.MultiReader

>`io.MultiReader` позволяет объединить несколько источников данных (reader-ов) в один, который будет читать данные поочередно из каждого источника.

**Пример использования `io.MultiReader`**:
```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	// Создаем несколько источников данных
	r1 := strings.NewReader("Hello, ")
	r2 := strings.NewReader("World!")
	r3 := strings.NewReader(" How are you?")

	// Создаем MultiReader
	multiReader := io.MultiReader(r1, r2, r3)

	// Читаем данные из объединенного источника
	buf := make([]byte, 1024)
	n, err := multiReader.Read(buf)
	if err != nil && err != io.EOF {
		fmt.Println("Error:", err)
	}
	fmt.Printf("Read %d bytes: %s\n", n, string(buf[:n]))
}
```

**Как это работает:**
- В примере данные из всех трех строк читаются по очереди (сначала из `r1`, потом из `r2`, и затем из `r3`).
- Это полезно, когда необходимо объединить несколько потоков данных и обрабатывать их как один.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go IO]]

------
**Links** (Внешние ссылки)
-
