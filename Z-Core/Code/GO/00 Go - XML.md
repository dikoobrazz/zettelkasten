2025012921:28
___
Date: 29-01-2025 | 21:28
Tags: #go #xml #decode #uncode
mapofcontents: [[zero-links|OO Links]]
___
## XML

### XML: Сериализация и десериализация, работа с тегами структур

**Что такое сериализация и десериализация?**
- **Сериализация** — это **преобразование** структуры данных в строку XML.
- **Десериализация** — это **разбор (парсинг)** XML обратно в структуру данных.

---
### XML в Go (encoding/xml)

Go также поддерживает XML с пакетом `encoding/xml`.

#### Сериализация в XML
```go
package main

import (
	"encoding/xml"
	"fmt"
)

type User struct {
	XMLName xml.Name `xml:"user"`
	ID      int      `xml:"id"`
	Name    string   `xml:"name"`
	Email   string   `xml:"email"`
}

func main() {
	user := User{ID: 1, Name: "Андрей", Email: "andrey@example.com"}

	xmlData, err := xml.MarshalIndent(user, "", "  ")
	if err != nil {
		fmt.Println("Ошибка сериализации:", err)
		return
	}

	fmt.Println(string(xmlData))
}
```

**Выходные данные:**
```xml
<user>
  <id>1</id>
  <name>Андрей</name>
  <email>andrey@example.com</email>
</user>
```

#### Десериализация XML в структуру
```go
package main

import (
	"encoding/xml"
	"fmt"
)

type User struct {
	ID    int    `xml:"id"`
	Name  string `xml:"name"`
	Email string `xml:"email"`
}

func main() {
	xmlString := `<user><id>1</id><name>Андрей</name><email>andrey@example.com</email></user>`

	var user User

	err := xml.Unmarshal([]byte(xmlString), &user)
	if err != nil {
		fmt.Println("Ошибка десериализации:", err)
		return
	}

	fmt.Println(user) // {1 Андрей andrey@example.com}
}
```

---

Вот таблица с **тегами для XML** в Go:

|XML-тег|Описание|
|---|---|
|`xml:"name"`|Использует `"name"` как название тега в XML.|
|`xml:"name,omitempty"`|Пропускает поле, если оно **пустое** (`""`, `0`, `nil`, `false`, `[]`, `map[]`).|
|`xml:"-"`|Игнорирует поле при сериализации и десериализации.|
|`xml:"name,attr"`|Сериализует поле как **атрибут** в XML.|
|`xml:"name,chardata"`|Сериализует поле как **текстовое содержимое** XML-тега.|
|`xml:"name,innerxml"`|Вставляет **сырой XML-код** внутрь тега.|
|`xml:",omitempty"`|Пропускает поле, если оно пустое, но **оставляет имя как есть**.|
|`xml:"name,omitempty,attr"`|Комбинирует `omitempty` и `attr`: атрибут **исчезает**, если пустой.|
|`xml:",chardata"`|Сериализует поле как **чистый текст**, без обёртки в тег.|
|`xml:",cdata"`|Заворачивает содержимое в `<![CDATA[...]]>`.|
|`xml:",comment"`|Сериализует поле как XML-комментарий (`<!-- ... -->`).|
|`xml:",any"`|Позволяет декодировать **любые вложенные теги** в `map[string]string`.|

---

**Пример использования тегов:**
```go
type User struct {
	XMLName xml.Name `xml:"user"`
	ID      int      `xml:"id,attr"`        // Атрибут
	Name    string   `xml:"name"`           // Обычное поле
	Email   string   `xml:"email,omitempty"` // Пропустится, если пустое
	Note    string   `xml:",chardata"`      // Вставится как текст в `<user>...</user>`
	Meta    string   `xml:",cdata"`         // Будет внутри `<![CDATA[...]]>`
}
```

**Выходные данные для разных значений**

|Поле|Значение в Go|XML без `omitempty`|XML с `omitempty`|
|---|---|---|---|
|`Name`|`"Андрей"`|`<name>Андрей</name>`|`<name>Андрей</name>`|
|`Name`|`""`|`<name></name>`|_Отсутствует_|
|`ID`|`25`|`<user id="25">`|`<user id="25">`|
|`Email`|`"test@mail.com"`|`<email>test@mail.com</email>`|`<email>test@mail.com</email>`|
|`Email`|`""`|`<email></email>`|_Отсутствует_|
|`Note`|`"Привет!"`|`<user>Привет!</user>`|`<user>Привет!</user>`|
|`Meta`|`"Hello"`|`<![CDATA[Hello]]>`|`<![CDATA[Hello]]>`|

---

**Пример XML-выхода**
**Входной объект:**
```go
user := User{
	ID:    123,
	Name:  "Андрей",
	Email: "",
	Note:  "Привет!",
	Meta:  "Секретное сообщение",
}
```

**Сгенерированный XML:**
```xml
<user id="123">
    <name>Андрей</name>
    Привет!
    <![CDATA[Секретное сообщение]]>
</user>
```

Эти теги помогают **оптимизировать XML-структуры** и **упрощают разбор данных**.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
