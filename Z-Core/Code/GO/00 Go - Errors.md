2025012920:44
___
Date: 29-01-2025 | 20:44
Tags: #go #errors #error
mapofcontents:
___
## Работа с ошибками в Go

Обработка ошибок в Go является ключевой частью программирования на этом языке. В отличие от языков с механизмами исключений (например, try/catch в Java, Python), Go использует **явное возвращение ошибок**.

Разберем основные механизмы обработки ошибок:

---

### 1. Возврат ошибок как значения

В Go принято, что функция возвращает ошибку в последнем возвращаемом значении:

```go
package main

import (
	"errors"
	"fmt"
)

// Функция, которая возвращает ошибку
func divide(a, b int) (int, error) {
	if b == 0 {
		return 0, errors.New("деление на ноль запрещено")
	}
	return a / b, nil
}

func main() {
	result, err := divide(10, 0)
	if err != nil {
		fmt.Println("Ошибка:", err)
	} else {
		fmt.Println("Результат:", result)
	}
}
```

- Функция `divide` возвращает два значения: результат вычислений и `error`.
- Если возникает ошибка (например, деление на 0), мы возвращаем `errors.New("...")`.
- В `main()` проверяем, была ли ошибка, и обрабатываем её.

**Правило**: Всегда проверяйте ошибку после вызова функции, если она возвращает `error`.

---

### 2. Использование `fmt.Errorf` для форматирования ошибок

Go предоставляет `fmt.Errorf` для добавления контекста в ошибки:

```go
package main

import (
	"fmt"
)

func openFile(filename string) error {
	return fmt.Errorf("не удалось открыть файл %s: %w", filename, err)
}

func main() {
	err := openFile("data.txt")
	if err != nil {
		fmt.Println("Ошибка:", err)
	}
}
```

Здесь `%w` позволяет **оборачивать ошибки**, чтобы их можно было анализировать позже.

---

### 3. Проверка ошибок с `errors.Is` и `errors.As`

Иногда важно понимать, является ли ошибка конкретным типом ошибки. Для этого используются `errors.Is()` и `errors.As()`.

#### 3.1. `errors.Is()` – сравнение ошибок

```go
package main

import (
	"errors"
	"fmt"
)

var ErrNotFound = errors.New("запись не найдена")

func findUser(id int) error {
	if id != 1 {
		return ErrNotFound
	}
	return nil
}

func main() {
	err := findUser(2)
	if errors.Is(err, ErrNotFound) {
		fmt.Println("Пользователь не найден")
	} else if err != nil {
		fmt.Println("Произошла другая ошибка:", err)
	}
}
```

Здесь `errors.Is(err, ErrNotFound)` проверяет, совпадает ли ошибка `err` с `ErrNotFound`.

---

#### 3.2. `errors.As()` – извлечение типа ошибки

Иногда нам нужно узнать **конкретный тип ошибки**:

```go
package main

import (
	"errors"
	"fmt"
)

// Кастомный тип ошибки
type MyError struct {
	Code int
	Msg  string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("код %d: %s", e.Code, e.Msg)
}

func doSomething() error {
	return &MyError{Code: 404, Msg: "страница не найдена"}
}

func main() {
	err := doSomething()

	var myErr *MyError
	if errors.As(err, &myErr) {
		fmt.Println("Ошибка с кодом:", myErr.Code)
	} else {
		fmt.Println("Произошла другая ошибка:", err)
	}
}
```

**Вывод**: `errors.As()` позволяет **распаковать** ошибку в конкретный тип.

---

### 4. Создание собственных типов ошибок

Можно создавать свои **структуры ошибок**, добавляя в них дополнительные данные:

```go
package main

import (
	"fmt"
)

// Определяем кастомную ошибку
type ValidationError struct {
	Field string
	Msg   string
}

// Реализация метода Error() для соответствия интерфейсу error
func (e *ValidationError) Error() string {
	return fmt.Sprintf("Ошибка в поле '%s': %s", e.Field, e.Msg)
}

func validate(input string) error {
	if input == "" {
		return &ValidationError{Field: "Username", Msg: "не может быть пустым"}
	}
	return nil
}

func main() {
	err := validate("")
	if err != nil {
		fmt.Println("Ошибка:", err)
	}
}
```

---

### 5. Пакет `log` для логирования ошибок

Для логирования ошибок можно использовать `log`:

```go
package main

import (
	"log"
	"os"
)

func main() {
	// Создание файла для логирования
	file, err := os.Open("nonexistent.txt")
	if err != nil {
		log.Fatalf("Ошибка открытия файла: %v", err)
	}
	defer file.Close()
}
```

- `log.Fatalf()` завершает выполнение программы после вывода сообщения.
- `log.Println()` просто записывает ошибку в лог.

---

### 6. Паника и восстановление (panic и recover)

В Go избегают `panic`, но в некоторых случаях её можно использовать.

#### 6.1. `panic` – аварийное завершение

```go
package main

import "fmt"

func main() {
	fmt.Println("Перед паникой")
	panic("что-то пошло не так")
	fmt.Println("Этот код не выполнится")
}
```

Когда вызывается `panic()`, программа сразу завершает выполнение.

---
#### 6.2. `recover` – перехват паники

Если нужно **перехватить** `panic` и продолжить выполнение, используем `recover()`.

```go
package main

import "fmt"

func safeFunction() {
	defer func() {
		if r := recover(); r != nil {
			fmt.Println("Перехвачена паника:", r)
		}
	}()
	
	panic("критическая ошибка!")
}

func main() {
	fmt.Println("Начало программы")
	safeFunction()
	fmt.Println("Программа продолжает работу")
}
```

**Когда использовать `panic` и `recover`?**
- **`panic`** подходит для фатальных ошибок (например, закрытие соединения).
- **`recover`** можно использовать для восстановления сервиса после ошибки.

---

### Выводы

1. В Go **ошибки возвращаются как значения** – `error`.
2. Используйте `errors.New()`, `fmt.Errorf()`, `errors.Is()` и `errors.As()` для работы с ошибками.
3. Можно создавать **кастомные ошибки** (структуры).
4. Логируйте ошибки с помощью `log`.
5. **Избегайте `panic`, но используйте `recover`, если необходимо обработать критические ошибки.**

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
