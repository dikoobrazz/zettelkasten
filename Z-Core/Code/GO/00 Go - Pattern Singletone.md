2025020402:58
___
Date: 04-02-2025 | 02:58
Tags: #go #pattern #singletone
mapofcontents: 
___
## Singletone Pattern (Одиночка)

### Когда использовать?

Когда нужно **гарантировать, что в системе будет только один экземпляр объекта**.
В Go реализуется через `sync.Once`, чтобы потокобезопасно инициализировать объект.

### Применение в Go:

- Управление подключением к базе данных.
- Логгеры.
- Конфигурация приложения.

**Пример Singleton в Go**
```go
package main

import (
	"fmt"
	"sync"
)

// Singleton структура
type Singleton struct {
	data string
}

var instance *Singleton
var once sync.Once

// Функция получения единственного экземпляра
func GetInstance() *Singleton {
	once.Do(func() {
		instance = &Singleton{data: "Я единственный экземпляр!"}
	})
	return instance
}

func main() {
	s1 := GetInstance()
	s2 := GetInstance()

	fmt.Println(s1.data) // "Я единственный экземпляр!"
	fmt.Println(s1 == s2) // true (оба указателя указывают на один объект)
}
```

**Отличие от других языков:**
- В Go **нет статических классов**, поэтому используем **глобальную переменную + sync.Once**.
- Это **потокобезопасный** способ создания синглтона.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
- 
