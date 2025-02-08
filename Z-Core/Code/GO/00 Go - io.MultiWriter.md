2025012922:35
___
Date: 29-01-2025 | 22:35
Tags: #go #io #multiwriter
mapofcontents: 
___
## io.MultiWriter

>`io.MultiWriter` позволяет записывать данные в несколько `io.Writer`-ов одновременно. Это удобно, когда нужно записывать данные сразу в несколько мест (например, в файл и в лог).

**Пример использования `io.MultiWriter`**:
```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	// Открываем файл для записи
	file, err := os.Create("output.txt")
	if err != nil {
		fmt.Println("Error creating file:", err)
		return
	}
	defer file.Close()

	// Создаем MultiWriter для записи в stdout и файл
	multiWriter := io.MultiWriter(os.Stdout, file)

	// Записываем данные в оба места
	_, err = multiWriter.Write([]byte("Hello, World!"))
	if err != nil {
		fmt.Println("Error writing:", err)
	}
}
```

 **Как это работает:**
- В этом примере, данные одновременно записываются как в консоль (`os.Stdout`), так и в файл `output.txt`.
- Это полезно, когда нужно сделать логирование или вывод информации сразу в несколько мест.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go IO]]

------
**Links** (Внешние ссылки)
-
