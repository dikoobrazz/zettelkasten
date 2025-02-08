2025012921:20
___
Date: 29-01-2025 | 21:20
Tags: #go #json #serialize #decode #encode
mapofcontents: [[zero-links|OO Links]]
___
## JSON

### JSON: Сериализация и десериализация, работа с тегами структур

**Что такое сериализация и десериализация?**
- **Сериализация** — это **преобразование** структуры данных в строку JSON/XML.
- **Десериализация** — это **разбор (парсинг)** JSON/XML обратно в структуру данных.

---

### 1. JSON в Go (encoding/json)

Go предоставляет стандартный пакет `encoding/json` для работы с JSON.

**Сериализация (структура → JSON)**
```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID    int    `json:"id"`
	Name  string `json:"name"`
	Email string `json:"email"`
}

func main() {
	user := User{ID: 1, Name: "Андрей", Email: "andrey@example.com"}

	// Преобразуем структуру в JSON
	jsonData, err := json.Marshal(user)
	if err != nil {
		fmt.Println("Ошибка сериализации:", err)
		return
	}

	fmt.Println(string(jsonData))   
	// {"id":1,"name":"Андрей","email":"andrey@example.com"}
}
```

**Десериализация (JSON → структура)**
```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	ID    int    `json:"id"`
	Name  string `json:"name"`
	Email string `json:"email"`
}

func main() {
	jsonString := `{"id":1,"name":"Андрей","email":"andrey@example.com"}`

	var user User

	// Преобразуем JSON в структуру
	err := json.Unmarshal([]byte(jsonString), &user)
	if err != nil {
		fmt.Println("Ошибка десериализации:", err)
		return
	}

	fmt.Println(user) // {1 Андрей andrey@example.com}
}
```

---

### JSON-теги в структурах

Теги управляют тем, как Go работает с JSON
Вот расширенная таблица с **JSON-тегами** в Go:

|JSON-тег|Описание|
|---|---|
|`json:"name"`|Использует `"name"` как ключ в JSON.|
|`json:"name,omitempty"`|Пропускает поле, если оно **пустое** (`""`, `0`, `nil`, `false`, `[]`, `map[]`).|
|`json:"-"`|Игнорирует поле при сериализации и десериализации.|
|`json:"name,string"`|Преобразует поле в **строку** (даже если это `int` или `bool`).|
|`json:"name,omitempty,string"`|Комбинация: если значение **не пустое**, сериализует как строку.|
|`json:"name,omitempty" xml:"name,attr"`|В JSON – пропускает пустые, в XML – сериализует как **атрибут**.|
|`json:",omitempty"`|Использует имя поля как есть, но **опускает** пустые значения.|
|`json:",string"`|Конвертирует значение в **строку** без переименования ключа.|
|`json:",omitempty,string"`|Убирает пустые поля и сериализует их как строки.|
|`json:"-" xml:"-"`|Полностью исключает поле из JSON и XML.|
|`json:"name,omitempty" bson:"name,omitempty"`|Используется и для **MongoDB (bson)**.|
|`json:"name,omitempty" form:"name"`|Используется и для **парсинга форм (form-urlencoded)**.|
|`json:"name,omitempty" query:"name"`|Используется для **парсинга URL-параметров**.|

**Примеры использования тегов:**
```go
type User struct {
	ID        int     `json:"id"`
	Name      string  `json:"name,omitempty"`
	Email     string  `json:"email"`
	Age       int     `json:"age,string"`      // Всегда сериализуется как строка
	Password  string  `json:"-"`               // Не попадёт в JSON
	Balance   float64 `json:"balance,omitempty,string"` // Пропускает пустые, но сериализует как строку
}
```

**JSON-выходные данные для разных значений**

|Поле|Значение в Go|JSON без `omitempty`|JSON c `omitempty`|
|---|---|---|---|
|`Name`|`"Андрей"`|`"name":"Андрей"`|`"name":"Андрей"`|
|`Name`|`""`|`"name":""`|_Отсутствует_|
|`Age`|`25`|`"age":25`|`"age":"25"` (из-за `string`)|
|`Age`|`0`|`"age":0`|`"age":"0"` (из-за `string`)|
|`Balance`|`0.0`|`"balance":0.0`|_Отсутствует_|
|`Password`|`"123456"`|_Отсутствует_|_Отсутствует_|

Эти теги помогают **оптимизировать JSON-ответы** и избегать лишних полей. 



-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
