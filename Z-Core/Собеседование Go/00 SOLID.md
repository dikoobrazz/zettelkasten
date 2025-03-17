2025030623:38
___
Date: 06-03-2025 | 23:38
Tags: #
mapofcontents: [[zero-links|OO Links]]
___

**SOLID** — это акроним из пяти принципов объектно-ориентированного проектирования, предложенных Робертом Мартином (Uncle Bob) для создания гибкого, поддерживаемого и масштабируемого кода. Вот краткое объяснение каждой буквы на уровне детализации 5/10:

---
### S — Single Responsibility Principle (Принцип единственной ответственности)

- **Что**: Каждый класс/модуль должен иметь только одну причину для изменения.
- **Пример**: Класс User только хранит данные пользователя, а не занимается их сохранением в базу.
- **Коротко**: Одна задача — один класс.

---
### O — Open/Closed Principle (Принцип открытости/закрытости)

- **Что**: Классы должны быть открыты для расширения (новая функциональность), но закрыты для модификации (старый код не меняется).
- **Пример**: Используем интерфейс Payment для добавления новых методов оплаты без изменения старого кода.
- **Коротко**: Расширяй, не ломай.

---
### L — Liskov Substitution Principle (Принцип подстановки Барбары Лисков)

- **Что**: Объекты базового класса можно заменить объектами производного без нарушения поведения программы.
- **Пример**: Если Bird имеет метод Fly(), то Penguin (наследник) не должен ломать логику, выбрасывая ошибку.
- **Коротко**: Заменяй без проблем.

---
### I — Interface Segregation Principle (Принцип разделения интерфейсов)

- **Что**: Клиенты не должны зависеть от интерфейсов, которые они не используют.
- **Пример**: Вместо одного большого Worker интерфейса создаем Eater и Sleeper отдельно.
- **Коротко**: Интерфейсы по делу.

---
### D — Dependency Inversion Principle (Принцип инверсии зависимостей)

- **Что**: Высокоуровневые модули не должны зависеть от низкоуровневых, оба должны зависеть от абстракций.
- **Пример**: Сервис Order зависит от интерфейса Database, а не от конкретной реализации MySQL.
- **Коротко**: Зависимости через абстракции.

---
### Коротко о SOLID

- **S**: Одна ответственность.
- **O**: Открыт для расширения, закрыт для изменений.
- **L**: Подстановка без поломок.
- **I**: Маленькие интерфейсы.
- **D**: Абстракции вместо конкретики.

---
## SOLID

> **SOLID** — это акроним, представляющий пять принципов объектно-ориентированного дизайна, которые помогают создавать гибкие, масштабируемые и поддерживаемые программы. Эти принципы были сформулированы Робертом Мартином (Uncle Bob) и широко применяются в разработке. В контексте Go, где нет классического ООП, SOLID адаптируется через структуры, интерфейсы и композицию

---
### 1. *S* — Single Responsibility Principle (Принцип единственной ответственности)

**Определение**: Класс (или структура в Go) должен иметь только одну причину для изменения, то есть выполнять одну задачу.

**Пример нарушения**:
```go
type User struct {
    Name  string
    Email string
}

func (u User) SaveToDB() {
    // Сохранение в базу данных
    println("Saving", u.Name, "to DB")
}

func (u User) SendEmail() {
    // Отправка email
    println("Sending email to", u.Email)
}
```

Здесь `User` отвечает и за сохранение в БД, и за отправку email — две разные ответственности.

**Исправление**:
```go
type User struct {
    Name  string
    Email string
}

type DB struct{}

func (db DB) Save(u User) {
    println("Saving", u.Name, "to DB")
}

type EmailService struct{}

func (es EmailService) Send(u User) {
    println("Sending email to", u.Email)
}

func main() {
    u := User{"Alice", "alice@example.com"}
    db := DB{}
    es := EmailService{}
    db.Save(u)
    es.Send(u)
}
```

Теперь каждая сущность отвечает за свою задачу.

---
### 2. *O* — Open/Closed Principle (Принцип открытости/закрытости)

**Определение**: Сущности (структуры, функции) должны быть открыты для расширения, но закрыты для модификации.

**Пример нарушения**:
```go
type Shape struct {
    Type  string
    Size  float64
}

func Area(s Shape) float64 {
    if s.Type == "circle" {
        return 3.14 * s.Size * s.Size
    } else if s.Type == "square" {
        return s.Size * s.Size
    }
    return 0
}
```

Добавление нового типа фигуры требует изменения функции `Area`.

**Исправление с интерфейсом**:
```go
type Shape interface {
    Area() float64
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return 3.14 * c.Radius * c.Radius
}

type Square struct {
    Side float64
}

func (s Square) Area() float64 {
    return s.Side * s.Side
}

func PrintArea(s Shape) {
    println(s.Area())
}

func main() {
    c := Circle{2}
    s := Square{3}
    PrintArea(c) // 12.56
    PrintArea(s) // 9
}
```

Теперь можно добавлять новые фигуры, не меняя `PrintArea`.

---
### 3. *L* — Liskov Substitution Principle (Принцип подстановки Барбары Лисков)

**Определение**: Объекты базового типа должны быть заменяемы объектами производного типа без нарушения поведения программы.

**Пример нарушения**:
```go
type Bird interface {
    Fly() string
}

type Sparrow struct{}

func (s Sparrow) Fly() string {
    return "Sparrow flies"
}

type Ostrich struct{}

func (o Ostrich) Fly() string {
    return "Ostrich can't fly" // Нарушение: страус не летает
}

func MakeBirdFly(b Bird) {
    println(b.Fly())
}
```

`Ostrich` нарушает ожидания интерфейса `Bird`.

**Исправление**:
```go
type Bird interface {
    Move() string
}

type Sparrow struct{}

func (s Sparrow) Move() string {
    return "Sparrow flies"
}

type Ostrich struct{}

func (o Ostrich) Move() string {
    return "Ostrich runs"
}

func MakeBirdMove(b Bird) {
    println(b.Move())
}

func main() {
    s := Sparrow{}
    o := Ostrich{}
    MakeBirdMove(s) // "Sparrow flies"
    MakeBirdMove(o) // "Ostrich runs"
}
```

Теперь поведение логично для всех типов.

---
### 4. *I* — Interface Segregation Principle (Принцип разделения интерфейсов)

**Определение**: Клиенты не должны зависеть от интерфейсов, которые они не используют.

**Пример нарушения**:
```go
type Worker interface {
    Work()
    Eat()
}

type Robot struct{}

func (r Robot) Work() {
    println("Robot works")
}

func (r Robot) Eat() {
    // Робот не ест, но вынужден реализовать метод
    println("Robot does not eat")
}
```

`Robot` не нуждается в `Eat`, но вынужден его реализовать.

**Исправление**:
```go
type Workable interface {
    Work()
}

type Eatable interface {
    Eat()
}

type Robot struct{}

func (r Robot) Work() {
    println("Robot works")
}

type Human struct{}

func (h Human) Work() {
    println("Human works")
}

func (h Human) Eat() {
    println("Human eats")
}

func main() {
    r := Robot{}
    h := Human{}
    var w Workable = r
    w.Work() // "Robot works"
    var e Eatable = h
    e.Eat()  // "Human eats"
}
```

Интерфейсы разделены, и `Robot` реализует только то, что нужно.

---
### 5. *D* — Dependency Inversion Principle (Принцип инверсии зависимостей)

**Определение**: Высокоуровневые модули не должны зависеть от низкоуровневых; оба должны зависеть от абстракций.

**Пример нарушения**:
```go
type SQLDatabase struct{}

func (db SQLDatabase) Save(data string) {
    println("Saving", data, "to SQL")
}

type UserService struct {
    db SQLDatabase // Прямая зависимость от конкретной реализации
}

func (us UserService) SaveUser(name string) {
    us.db.Save(name)
}
```

**Исправление**:
```go
type Database interface {
    Save(data string)
}

type SQLDatabase struct{}

func (db SQLDatabase) Save(data string) {
    println("Saving", data, "to SQL")
}

type UserService struct {
    db Database // Зависимость от интерфейса
}

func (us UserService) SaveUser(name string) {
    us.db.Save(name)
}

func main() {
    db := SQLDatabase{}
    us := UserService{db: db}
    us.SaveUser("Alice") // "Saving Alice to SQL"
}
```

Теперь `UserService` зависит от абстракции (Database), а не от конкретной реализации.

---
### Итог

- **SRP**: Одна ответственность — одна сущность.
- **OCP**: Расширяйте через интерфейсы, а не изменения.
- **LSP**: Подтипы должны соответствовать ожиданиям базового типа.
- **ISP**: Интерфейсы должны быть узкими и специфичными.
- **DIP**: Зависимости через абстракции.

В Go SOLID реализуется через интерфейсы и композицию, а не через классы и наследование. Эти принципы помогают писать чистый и модульный код.



-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
