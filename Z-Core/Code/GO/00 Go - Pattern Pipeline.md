2025020415:45
___
Date: 04-02-2025 | 15:45
Tags: #go #pattern #pipeline
mapofcontents:
___
## Pattern Pipeline (Конвейер)

### Когда использовать?

Когда нужно **обрабатывать поток данных, разбивая его на последовательные шаги**.
В Go это удобно реализуется через **каналы (channels)**.
### Применение в Go:

- Обработка данных в потоках.
- Фильтрация и преобразование данных.

**Пример Pipeline в Go**
```go
package main

import (
	"fmt"
	"strings"
)

// Шаг 1: Читаем данные
func generateData(out chan<- string) {
	defer close(out)
	data := []string{"hello", "world", "golang"}
	for _, word := range data {
		out <- word
	}
}

// Шаг 2: Преобразуем данные
func toUpperCase(in <-chan string, out chan<- string) {
	defer close(out)
	for word := range in {
		out <- strings.ToUpper(word)
	}
}

// Шаг 3: Выводим результат
func printResults(in <-chan string) {
	for word := range in {
		fmt.Println(word)
	}
}

func main() {
	ch1 := make(chan string)
	ch2 := make(chan string)

	go generateData(ch1)     // Источник данных
	go toUpperCase(ch1, ch2) // Обработка данных
	printResults(ch2)        // Вывод данных
}
```

**Отличие от других языков:**
- В Go **pipeline естественно реализуется через каналы**.
- Можно **легко распараллелить** обработку данных (`go toUpperCase(...)`).

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
-
