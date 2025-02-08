2025012922:13
___
Date: 29-01-2025 | 22:13
Tags: #go #io #writer
mapofcontents: [[zero-links|OO Links]]
___
## io.Writer

>`io.Writer` — это интерфейс, который определяет метод `Write(p []byte) (n int, err error)` Этот интерфейс используется для записи данных в целевой поток. Метод `Write` записывает байты из переданного среза и возвращает количество записанных байт и возможную ошибку.

**Основные моменты:**
- `Write` записывает данные из среза `p` в целевой поток.
- Возвращает количество записанных байт и ошибку.
- Ошибка может быть `io.EOF`, если поток не может принять больше данных.

**Пример использования `io.Writer`**:
```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	// Создаем файл для записи
	file, err := os.Create("output.txt")
	if err != nil {
		fmt.Println("Error creating file:", err)
		return
	}
	defer file.Close()

	// Используем io.Writer для записи данных в файл
	var writer io.Writer = file

	// Записываем данные в файл
	n, err := writer.Write([]byte("Hello, Writer!"))
	if err != nil {
		fmt.Println("Error writing:", err)
	}
	fmt.Printf("Wrote %d bytes\n", n)
}
```

**Как это работает:**
- Мы создаем новый файл `output.txt` с помощью `os.Create`, который возвращает объект, реализующий интерфейс `io.Writer`.
- Используем метод `Write`, чтобы записать строку "Hello, Writer!" в файл.
- В конце выводим количество записанных байт.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go IO]]

------
**Links** (Внешние ссылки)
-
