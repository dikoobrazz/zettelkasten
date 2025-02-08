 2023062010:56
___
Date: 20-06-2023 | 10:56
Tags: #code #go #lesson
mapofcontents: 
___
# Lessons Notes

## Lesson 1. Intro

> Модель языка Go НЕ основана на виртуальной машине как Java, C#, Ruby
> Аппаратное обеспечение является платформой для программ на Go
> Именно аппаратное обеспечение определяет, как работает наша программа
> Go помогает использовать всё приемущество современного оборудования

> Каждое решение в программировании имеет свою цену, ничего не бывает бесплатно
> Либо мы имеем скорость, но теряем в памяти и наооборот. Это и есть программная инженерия
> Увеличение количества аппаратного обеспечения и облачных сервисов уже не является решением проблемы в программировании

## Lesson 2

### Variables. Foundation

> Тип - это жизнь.
> Поскольку тип предоставляет компилятор, он предоставляет нам две части важной информации
> 1. Oбъем памяти, который нам нужно выделить 
> 2. Что эта память представляет

`var c float64` - требует 64 бит или 8 байтов выделенной памяти и представляет собой двоично-десятичный формат IEEE 754.
`var d bool` - это один байт или 8 бит выделенной памяти и представляет true или false

В Go есть **int8**, **int16**, **int32**, **int64**.
Итак, тип **int** не имеет размера. Каков размер **int** в Go? Откуда мы знаем, во что нам обходится это целое число? 
Размер **int** зависит от архетиктуры процессора. AMD64 - является 64-битной архитектурой. Это означает, что мы используем 64-битные адреса.

`var b string` - представляет собой структуру данных из двух слов (|byte (nil)|lenght (0)|)
![[_resources/strings_GO.png|200]]

`a := int32(10)`
**В Go нет "Кастинга" (Cast - приведение типа). 
В Go есть "Конверсия" (Conversion - преобразование типа).**
Преобразование типа может привести к дополнительному распределению

`bb := "hello"`
![[_resources/strings_GO_2.png|300]]

### Struct Types

> В Go нет ключевого слова **class**. В Go есть ключевое слово **struct**.
> **struct** - собственные пользовательские типы в Go.

```Go
package main

type example struct {
	flag    bool
	counter int16
	pi      float32
}

func main() {
	var e1 exapmple
}
```

Объявленна переменная **e1** типа **example** 
Какое её **zero value**? Сколько памяти выделяется?
* bool - 1 byte
* int16 - 2 byte
* float32 - 4 byte
  ~~**7 byte?**~~  -> **8 byte**
В Go работает так называемое "выравнивание"

>  В зависимости от размера конкретно значения, Go определит выравнивание, которое необходимо
>  Каждая следующая граница разделения должна приходиться кратное 2 байтам

![[_resources/struct_GO_2.png]]

###### Для минимизации выравнивания в Go определять поля структуры от большего к меньшему!!!
```Go
package main

type example struct {
	id      int64
	pi      float32
	counter int16
	flag    bool
}

func main() {
	var e1 exapmple
}
```


```Go
package main

type bob struct {
	id      int64
	pi      float32
	counter int16
	flag    bool
}

type lisa struct {
	id      int64
	pi      float32
	counter int16
	flag    bool
}

func main() {
	var b bob
	var l lisa
	l = b
	fmt.Println(b, l) // Error!!!

	l = lisa(b)
	fmt.Println(b, l) // Ok!
	
	e2 := struct {
		id      int64
		pi      float32
		counter int16
		flag    bool
	}{
		id: 1,
		pi: 3.14,
		counter: 8,
		flag: true,
	}

	l = e2
	fmt.Println(l , e2) // Ok!
}
```

- `bob` и `lisa` — это два разных типа, даже если они имеют одинаковые поля.
- Go не позволяет напрямую присваивать значение одного типа другому, даже если их структура идентична.

---
#### Почему работает преобразование типов?

Когда вы используете **преобразование типов (type conversion)**, Go проверяет, что структуры имеют одинаковый набор полей с одинаковыми типами и порядком. Если это так, преобразование разрешено.

`l = lisa(b) // Преобразование типа bob в lisa`

Здесь Go видит, что `bob` и `lisa` имеют одинаковую структуру, и позволяет выполнить преобразование.

---
#### Почему работает присваивание анонимной структуры?

Анонимная структура (например, `e2`) может быть присвоена переменной типа `lisa`, если их поля совпадают по типам и порядку. Это работает, потому что Go проверяет структуру, а не имя типа.

```go
e2 := struct {
    id      int64
    pi      float32
    counter int16
    flag    bool
}{
    id:      1,
    pi:      3.14,
    counter: 8,
    flag:    true,
}

l = e2 // Присваивание анонимной структуры
```

Здесь Go видит, что `e2` и `lisa` имеют одинаковую структуру, и позволяет выполнить присваивание.

#### Правила присваивания и преобразования структур

1. **Прямое присваивание**:
    - Разрешено только между переменными одного и того же типа.
2. **Преобразование типов**:
	- Разрешено, если структуры имеют одинаковый набор полей с одинаковыми типами и порядком.
3. **Присваивание анонимной структуры**:
	- Разрешено, если анонимная структура и целевой тип имеют одинаковый набор полей с одинаковыми типами и порядком.

---
#### Итог
- В Go структуры с одинаковыми полями, но разными именами, считаются разными типами.
- Прямое присваивание между разными типами структур запрещено.
- Преобразование типов (`lisa(b)`) разрешено, если структуры идентичны.
- Присваивание анонимной структуры разрешено, если её поля совпадают с целевым типом.

___
### Pointers

> В Go - всё передаётся по значению.
> Указатели предназначены для совместного использования значения.

> Ссылочные типы в Go:
> 1. slice
> 2. map
> 3. inteface value
> 4. function value

### Constants

### Functions

## Lesson 3. Data Structure

### Arrays

> Массив является самой важной структурой днных ->
> -> Массив является непрерывным блоком памяти ->
> -> Оборудование любит массивы за предсказуемый шаблон доступа к данным

> В Go массивы имеют исвестный размер времени компиляции. Следовательно...
> В Go массивы не могут быть переменнными, а только константами
> Это означает, что массив может найти себя в стеке(stack), а не в куче(heap)

`var strings [5]string`
![[_resources/array_GO.png|250]]
`strings[0] = "apple"`
![[_resources/array_GO_2.png]]

```Go
for i, f := range strings {
	fmt.println(f)
}
```
![[_resources/array_GO_4.png]]

### Slice

> В Go срез (slice) - является самой важной структурой днных 
> В большинстве случаев это самая практичная структура данных, которую нужно использовать
> Slice в Go базируется на массиве

`slice := make([]string, 5)`
> `make` - встроенная функция
> `make` - работает только со срезами(slice), катрами(map) и каналами(chanel)

![[_resources/slice_GO.png|400]]

> Len - (длина) количество элементов к которым у нас есть доступ (для чтения и записи)
> Cap - (ёмкость) общее количество элементов в резервном массиве на котором основан срез

`slice[0] = "apple"`
![[_resources/slice_GO_2.png|400]]

`slice[5] = "Runtime error"` - Error: panic: runtime error: index out of range
`slice = append(slice, "Runtime err")` - Ok!

**Разница в объявлении среза (ссылочных типов)**
```Go
var data []string
data := []string{}
```
![[_resources/slice_GO_4.png|200]]

```Go
slice := make([]string, 5, 8)
slice[0] = "Apple"
slice[1] = "Orange"
slice[2] = "Banana"
slice[3] = "Grape"
slice[4] = "Plum"
```
![[_resources/slice_GO_3.png|400]]

`slice2 := slice[2:4]`
![[_resources/slice_GO_5.png|400]]

```Go
slice2[1] = "CHANGED"

fmt.Println(slice)
fmt.Println(slice2)

// slice
"Apple"
"Orange"
"Banana"
"CHANGED"
"Plum"

// slice2
"Banana"
"CHANGED"
```
![[_resources/slice_GO_6.png|400]]

```Go
slice2 = append(slice2, "CHANGED")

fmt.Println(slice)
fmt.Println(slice2)

// slice
"Apple"
"Orange"
"Banana"
"Grape"
"CHANGED"

// slice2
"Banana"
"Grape"
"CHANGED"
```
![[_resources/slice_GO_7.png|400]]

```Go
slice2 := slice[2:4:4] <-----
slice2 = append(slice2, "CHANGED")

fmt.Println(slice)
fmt.Println(slice2)

// slice
"Apple"
"Orange"
"Banana"
"Grape"
"Plum"

// slice2
"Banana"
"Grape"
"CHANGED"
```

### Map

```Go
type user struct {
	name string
	surname string
}

users := make(map[string]user)
users["Roy"] = user{"Rob", "Roy"}
users["Ford"] = user{"Henry", "Ford"}
users["Mouse"] = user{"Mickey", "Mouse"}
users["Jackson"] = user{"Michael", "Jackson"}

users2 := map[string]user {
	"Roy": {"Rob", "Roy"},
	"Ford": {"Henry", "Ford"},
	"Mouse": {"Mickey", "Mouse"},
	"Jackson": {"Michael", "Jackson"},
}

delete(users, "Roy") // delete the Roy key

u, found := users["Roy"]
fmt.Println("Roy:", u, found) // Roy false {}


```

## Lesson 4. Decoupling

### Methods

> Метод - простая функция которая имеет параметр (приемника) reciever

```Go
type user struct {
	name string
	email string
}

// value reciever method
// приемник (reciever) значений
func (u user) notify() {
	fmt.Printf("Sending User Email To %s<%s>\n", n.name, n.email)
}

// pointer reciever method
// приемник (reciever) указателей
func (u *user) changeEmail(email string) {
	u.email = email
}

func main() {
	bill := user{"Bill", "bill@email.com"}
	bill.notify()

	lisa := &user{"Lisa", "lisa@email.com"}
	lisa.notify()

	bill.changeEmail("bill@hotmail.com")
	bill.notify()

	lisa.changeEmail("lisa@hotmail.com")
	lisa.notify()
}
```

> Никогда не смешивайте в коде оба вида методов value reciever и pointer reciever
> Решение о выборе вида методов должно иcходить из самого типа структуры

> Когда мы работаем со встроенными типами в Go (int, string, bool, etc...) примитивными данными, мы всегда должны использовать семантику значений (values)
> Это так же касается и встроенных ссылочных типах (slice, map, chanel, interface)
> ==ИСКЛЮЧЕНИЕ== - slice unmarshal function. Для unmarshal требуется адрес

> Когда мы работаем с типом структур - нужно принять решение какую семантику использовать.
> Если не уверен на 100% - используй семантику указателя (pointers)
> Всегда безопастнее поделиться ссылкой на значение

```Go
type data struct {
	name string
	age int
}

func (d data) displayName() {
	fmt.Println("My name is", d.name)
}

func (d *data) setAge(age int) {
	d.age = age
	fmt.Println(d.name, "Is age", d.age)
}

func main() {
	d := data{
		name: "Bill",
	}

	d.displayName()
	d.setAge(42)

	data.displayName(d)
	(*data).setAge(&d, 42)
}
```

### Interfaces

> Interface - не является структурным типом, а является типом интрефейса
> Interface - не объявляет состояние, он определяет поведение
> Через контракт поведения мы можем иметь полиморфизм

> Конкретный тип - то любой тип, который может иметь метод

`var r interface{}`
![[_resources/interfaces_GO.png|100]]

.... TODO

```Go
type user struct {
	name string
	email string
}

func (u *user) String() string {
	return fmt.Sprintf("My name is %q and my emmail is %q", u.name, u.email)
}

func main() {
	u := user{
		name: "Bill",
		email: "bill@mail.com",
	}

	fmt.Println(u)  // {Bill bill@mail.com}
	fmt.Println(&u) // My name is "Bill" and my emmail is "bill@mail.com"
}
```


```Go
type printer interface {
	print()
}

type user struct {
	name string
}

// value reciever
func (u user) print() {
	fmt.Printf("User name: %s\n", u.name)
}

func main() {
	u := user{"Bill"}
	entities := []printer{
		u,
		&u,
	}

	u.name = "Bill_CHG"
	for _, e := range entities {
		e.print()
	}
	// User name: Bill
	// User name: Bill_CHG
}
```
![[_resources/interfaces_GO_2 1.png|300]]

### Embedding

```Go
type user struct {
	name  string
	email string
}

func (u *user) notify() {
	fmt.Printf("Sending user email to %s<%s>\n", u.name, u.email)
}

type admin struct {
	person user
	level  string
}

func main() {
	ad := admin{
		person: user{"Bill", "bill@mail.com"},
		level:  "super",
	}
	ad.person.notify()
}
```
[PlayGround](https://go.dev/play/p/kVHy_Psdn-1)

**Embeded Type**
```Go
type user struct {
	name  string
	email string
}

func (u *user) notify() {
	fmt.Printf("Sending user email to %s<%s>\n", u.name, u.email)
}

type admin struct {
	user  // embeded type
	level string
}

func main() {
	ad := admin{
		user:  user{"Bill", "bill@mail.com"},
		level: "super",
	}
	ad.user.notify()
	ad.notify()
}
```
[PlayGround](https://go.dev/play/p/grBteNq3_dx)

**Embeded Type with Interface**
```Go
type notifier interface {
	notify()
}

type user struct {
	name  string
	email string
}

func (u *user) notify() {
	fmt.Printf("Sending user email to %s<%s>\n", u.name, u.email)
}

type admin struct {
	user  // embeded type
	level string
}

func sendNotification(n notifier) {
	n.notify()
}

func main() {
	ad := admin{
		user:  user{"Bill", "bill@mail.com"},
		level: "super",
	}
	sendNotification(&ad)
}
```
[PlayGround](https://go.dev/play/p/U7uD1d1gtq6)

**Embeded Type with Interface overload**
```Go
type notifier interface {
	notify()
}

type user struct {
	name  string
	email string
}

func (u *user) notify() {
	fmt.Printf("Sending user email to %s<%s>\n", u.name, u.email)
}

type admin struct {
	user  // embeded type
	level string
}

func (a *admin) notify() {
	fmt.Printf("Sending admin email to %s<%s>\n", a.name, a.email)
}

func sendNotification(n notifier) {
	n.notify()
}

func main() {
	ad := admin{
		user:  user{"Bill", "bill@mail.com"},
		level: "super",
	}
	sendNotification(&ad)
	ad.user.notify()
	ad.notify()
}
```
[PlayGround](https://go.dev/play/p/Z6lswriVGHj)

## Lesson 5. Composition
#### Interface and Composition Design

> Композиция выходит за рамки механики встраивания типов и представляет собой нечто большее, чем просто парадигму. Это ключ к поддержанию стабильности вашего программного обеспечения за счет возможности адаптироваться к предстоящим изменениям данных и трансформации.

### Grouping Types

> Короче GO не будет группировать подтипы у которых Один тип наследования
> т.е есть общий тип Animal и наследники -> DOG -> CAT в языке GO так не сработает
> _"Подтип не способствует разнообразию"_
> Не нужно группирвать по ДНК - группируй по ПОВЕДЕНИЮ
> Иначе... группируй не по общей struct а по общему interface
> Перестань думать о БАЗОВОМ типе - это не Java!!!
> Компилятор определяет соответствие интрефейса!

**Пример НЕправильно**
```Go
package main

import "fmt"

type Animal struct {
	Name     string
	IsMammal bool
}

func (a *Animal) Speak() {
	fmt.Printf(
		"UGH! My name is %s, it is %t I am a mammal\n",
		a.Name,
		a.IsMammal,
	)
}

type Dog struct {
	Animal
	PackFactor int
}

func (d *Dog) Speak() {
	fmt.Printf(
		"Woof! My name is %s, it is %t I am a mammal with a pack factor of %d.\n",
		d.Name,
		d.IsMammal,
		d.PackFactor,
	)
}

type Cat struct {
	Animal
	ClimbFactor int
}

func (c *Cat) Speak() {
	fmt.Printf(
		"Meow! My name is %s, it is %t I am a mammal with a climb factor of %d.\n",
		c.Name,
		c.IsMammal,
		c.ClimbFactor,
	)
}

func main() {

	animals := []Animal{

		Dog{
			Animal: Animal{
				Name:     "Fido",
				IsMammal: true,
			},
			PackFactor: 5,
		},

		Cat{
			Animal: Animal{
				Name:     "Milo",
				IsMammal: true,
			},
			ClimbFactor: 4,
		},
	}

	for _, animal := range animals {
		animal.Speak()
	}
}
```
[PlayGround](https://go.dev/play/p/Dh_cCEz3o0N)

**Пример ПРАВИЛЬНО**
```Go
package main

import "fmt"

type Speaker interface {
	Speak()
}

type Dog struct {
	Name       string
	IsMammal   bool
	PackFactor int
}

func (d *Dog) Speak() {
	fmt.Printf(
		"Woof! My name is %s, it is %t I am a mammal with a pack factor of %d.\n",
		d.Name,
		d.IsMammal,
		d.PackFactor,
	)
}

type Cat struct {
	Name        string
	IsMammal    bool
	ClimbFactor int
}

func (c *Cat) Speak() {
	fmt.Printf(
		"Meow! My name is %s, it is %t I am a mammal with a climb factor of %d.\n",
		c.Name,
		c.IsMammal,
		c.ClimbFactor,
	)
}

func main() {

	speakers := []Speaker{

		&Dog{
			Name:       "Fido",
			IsMammal:   true,
			PackFactor: 5,
		},

		&Cat{
			Name:        "Milo",
			IsMammal:    true,
			ClimbFactor: 4,
		},
	}

	for _, spkr := range speakers {
		spkr.Speak()
	}
}
```
[PlayGround](https://go.dev/play/p/wRpHBoPu79K)

### Decoupling

> Сначала пишем рабочий код - основу! Тупо, Грубо - НО рабочий код!
> Разработай план и решай одну проблему за раз!

> Функция всегда лучше метода, если не нужно реализовывать интерфейс
> Функция более читабельна. Когда пишем функцию все входные данные должны быть переданы
> Когда используем Метод, сигнатура метода не указывает ни на одном уровне, какие поля и состояния мы используем для вызова, вся информация скрыта.
> С функциями код становиться более читабельным

```Go
import (
	"errors"
	"fmt"
	"io"
	"math/rand"
	"time"
)

type Data struct {
	Line string
}

type Xenia struct {
	Host    string
	Timeout time.Duration
}

func (*Xenia) Pull(d *Data) error {
	switch rand.Intn(10) {
	case 1, 9:
		return io.EOF
	case 5:
		return errors.New("error reading data from xenia")
	default:
		d.Line = "Data"
		fmt.Println("In:", d.Line)
		return nil
	}
}

type Pillar struct {
	Host    string
	Timeout time.Duration
}

func (*Pillar) Store(d *Data) error {
	fmt.Println("Out:", d.Line)
	return nil
}

type System struct {
	Xenia
	Pillar
}

func pull(x *Xenia, data []Data) (int, error) {
	for i := range data {
		if err := x.Pull(&data[i]); err != nil {
			return i, err
		}
	}
	return len(data), nil
}

func store(p *Pillar, data []Data) (int, error) {
	for i := range data {
		if err := p.Store(&data[i]); err != nil {
			return i, err
		}
	}
	return len(data), nil
}

func Copy(sys *System, batch int) error {
	data := make([]Data, batch)
	for {
		i, err := pull(&sys.Xenia, data)
		if i > 0 {
			if _, err := store(&sys.Pillar, data[:i]); err != nil {
				return err
			}
		}
		if err != nil {
			return err
		}
	}
}

func main() {
	sys := System{
		Xenia:  Xenia{},
		Pillar: Pillar{},
	}

	if err := Copy(&sys, 3); err != nil {
		fmt.Println(err)
	}
}
```
[PlayGround](https://go.dev/play/p/66afwVAilaO)

**Рефакторинг кода с условием будущих бизнес - изменений и дополнений**
```Go
import (
	"errors"
	"fmt"
	"io"
	"math/rand"
	"time"
)

type Data struct {
	Line string
}

type Puller interface {
	Pull(d *Data) error
}

type Storer interface {
	Store(d *Data) error
}

type PullStorer interface {
	Puller
	Storer
}

type Xenia struct {
	Host    string
	Timeout time.Duration
}

func (*Xenia) Pull(d *Data) error {
	switch rand.Intn(10) {
	case 1, 9:
		return io.EOF
	case 5:
		return errors.New("error reading data from xenia")
	default:
		d.Line = "Data"
		fmt.Println("In:", d.Line)
		return nil
	}
}

type Pillar struct {
	Host    string
	Timeout time.Duration
}

func (*Pillar) Store(d *Data) error {
	fmt.Println("Out:", d.Line)
	return nil
}

type System struct {
	Puller
	Storer
}

func pull(p Puller, data []Data) (int, error) {
	for i := range data {
		if err := p.Pull(&data[i]); err != nil {
			return i, err
		}
	}
	return len(data), nil
}

func store(s Storer, data []Data) (int, error) {
	for i := range data {
		if err := s.Store(&data[i]); err != nil {
			return i, err
		}
	}
	return len(data), nil
}

func Copy(ps PullStorer, batch int) error {
	data := make([]Data, batch)
	for {
		i, err := pull(ps, data)
		if i > 0 {
			if _, err := store(ps, data[:i]); err != nil {
				return err
			}
		}
		if err != nil {
			return err
		}
	}
}

func main() {
	sys := System{
		Puller: &Xenia{},
		Storer: &Pillar{},
	}

	if err := Copy(&sys, 3); err != nil {
		fmt.Println(err)
	}
}
```
[PlayGround](https://go.dev/play/p/Rbaoqr1ztS9)

### Conversion and Assertions (преобразование и утверждение)

**Приведение типов** - (плохая примета, следует пересмотреть код)
```Go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

type car struct{}

// String реализует интерфейс fmt.Stringer.
func (car) String() string {
	return "Vroom!"
}

type cloud struct{}

// String реализует интерфейс fmt.Stringer.
func (cloud) String() string {
	return "Big Data!"
}

func main() {

	// Seed the number random generator.
	rand.Seed(time.Now().UnixNano())

	// Create a slice of the Stringer interface values.
	mvs := []fmt.Stringer{
		car{},
		cloud{},
	}

	for i := 0; i < 10; i++ {

		// Choose a random number from 0 to 1.
		rn := rand.Intn(2)

		// приведение типа mvs[rn].(cloud)
		if v, is := mvs[rn].(cloud); is {
			fmt.Println("Got Lucky:", v)
			continue
		}

		fmt.Println("Got Unlucky")
	}
}
```
[PlayGround](https://go.dev/play/p/PtdQOc9xZ7S)

### Interface Pollution (загрязнение интерфейса)

> Используй интерфейсы, когда это целесообразно. Не забываем про стоимость интерфейсов.
> Не используй интерфейсы лишь ради тестов. Займись дизайном API!

**Пример загрязнения интерфейса**
_Классический пример разработки ПО от интерфейса в низ_
```Go
package main

// Server определяет контракт для TCP-серверов
// Название интерфейса не определяет его действия.
type Server interface {
	Start() error
	Stop() error
	Wait() error
}

// server - это наша реализация Server
type server struct {
	host string

}

// NewServer rвозвращает значение интерфейса типа Server с реализацией server
// ! Возвращает интерфейс - плохо. Итерфейс подаётся обычно на вход
func NewServer(host string) Server {

	// SMELL - Сохранение указателя неэкспортированного типа в интерфейсе.
	return &server{host}
}

// Start allows the server to begin to accept requests.
func (s *server) Start() error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}

// Stop shuts the server down.
func (s *server) Stop() error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}

// Wait prevents the server from accepting new connections.
func (s *server) Wait() error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}

func main() {

	// Create a new Server.
	srv := NewServer("localhost")

	// Use the API.
	srv.Start()
	srv.Stop()
	srv.Wait()
}

// Notes:
// * Пакет объявляет интерфейс, который соответствует всему API своего конкретного типа. 
// * Интерфейс экспортируется, но конкретный тип не экспортируется. 
// * Фабричная функция возвращает значение интерфейса с неэкспортированным значением конкретного типа внутри. 
// * Интерфейс можно удалить и для пользователя API ничего не изменится. 
// * Интерфейс не отделяет API от изменений.
```
[PlayGround](https://go.dev/play/p/DCqTbY14loz)

**Пример правильного решения, дизайна**
```Go
package main

// Server is our Server implementation.
type Server struct {
	host string

	// PRETEND THERE ARE MORE FIELDS.
}

func NewServer(host string) *Server {

	return &Server{host}
}

// Start allows the server to begin to accept requests.
func (s *Server) Start() error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}

// Stop shuts the server down.
func (s *Server) Stop() error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}

// Wait prevents the server from accepting new connections.
func (s *Server) Wait() error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}

func main() {

	// Create a new Server.
	srv := NewServer("localhost")

	// Use the API.
	srv.Start()
	srv.Stop()
	srv.Wait()
}
```
[PlayGround](https://go.dev/play/p/K3w2eX7V1j2)

**Рекомендации За и Против использованя интерфейса**
**ЗА:**
- Когда пользователям API необходимо предоставить подробную информацию о реализации. 
- Когда API имеет несколько реализаций, которые необходимо поддерживать. 
- Когда части API, которые могут измениться, идентифицированы и требуют разделения.
**Против:**
- Когда его единственной целью является написание тестируемых API (сначала напишите пригодные для использования API). 
- Когда он не обеспечивает поддержку API для отделения от изменений. 
- Когда не понятно, как интерфейс делает код лучше.

##### НЕ пиши API с интерфейсом ради тестов! Пиши код ради пользователей!

### Mocking

> Не используй mocking над базой данных - используй Docker
> Не используй интерфейс ради тестов и mocking! Ставь чистоту кода на первое место!
> Используй интерфейс только когда он делает твой API лучше!
> Не беспокойся о пользовательских тестах твоего API, пусть мокают твой API сами.

**Пример твоего API без интерфейса**
```Go
package pubsub

// PubSub provides access to a queue system.
type PubSub struct {
	host string

	// PRETEND THERE ARE MORE FIELDS.
}

// New creates a pubsub value for use.
func New(host string) *PubSub {
	ps := PubSub{
		host: host,
	}

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.

	return &ps
}

// Publish sends the data for the specified key.
func (ps *PubSub) Publish(key string, v interface{}) error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}

// Subscribe sets up an request to receive messages for the specified key.
func (ps *PubSub) Subscribe(key string) error {

	// PRETEND THERE IS A SPECIFIC IMPLEMENTATION.
	return nil
}
```
[PlayGround](https://go.dev/play/p/299EFra4b4z)

**Пример клиентского кода**
```Go
package main

import (
	"github.com/ardanlabs/gotraining/topics/go/design/composition/mocking/example1/pubsub"
)

// publisher is an interface to allow this package to mock the pubsub
// package support.
type publisher interface {
	Publish(key string, v interface{}) error
	Subscribe(key string) error
}

// mock is a concrete type to help support the mocking of the pubsub package.
type mock struct{}

// Publish implements the publisher interface for the mock.
func (m *mock) Publish(key string, v interface{}) error {

	// ADD YOUR MOCK FOR THE PUBLISH CALL.
	return nil
}

// Subscribe implements the publisher interface for the mock.
func (m *mock) Subscribe(key string) error {

	// ADD YOUR MOCK FOR THE SUBSCRIBE CALL.
	return nil
}

func main() {

	// Create a slice of publisher interface values. Assign
	// the address of a pubsub.PubSub value and the address of
	// a mock value.
	pubs := []publisher{
		pubsub.New("localhost"),
		&mock{},
	}

	// Range over the interface value to see how the publisher
	// interface provides the level of decoupling the user needs.
	// The pubsub package did not need to provide the interface type.
	for _, p := range pubs {
		p.Publish("key", "value")
		p.Subscribe("key")
	}
}
```
[PlayGround](https://go.dev/play/p/-_laMS2yxZB)

### Design Guidelines (рекомендации по проектированию)

#### Interface And Composition Design

> Итерфейс даёт структуру твоей программе и поощеряет дизайн через композицию.
> Нельзя начинать писать код сверху вниз, нужно начинать писать с низу вверх.
> Нужно решить основные задачи слой за слоем, структура будет естественной, и решит основную задачу для продакшена.
> Декомпозиция означает уменьшение зависимости между компонентами.

**Философия дизайна:**
- Интерфейсы придают программам структуру. 
- Интерфейсы поощряют дизайн по композиции. 
- Интерфейсы позволяют и обеспечивают четкое разделение между компонентами. 
	- Стандартизация интерфейсов может установить четкие и последовательные ожидания. 
- Разделение означает уменьшение зависимостей между компонентами и типами, которые они используют. 
	- Это приводит к правильности, качеству и производительности. 
- Интерфейсы позволяют группировать конкретные типы по тому, что они делают. 
	- Группируйте типы не по общей ДНК, а по общему поведению. 
	- Каждый может работать вместе, если мы сосредоточимся на том, что мы делаем, а не на том, кем мы являемся. 
- Интерфейсы помогают вашему коду отделиться от изменений. 
	- Вы должны сделать все возможное, чтобы понять, что может измениться, и использовать интерфейсы для разделения. 
	- Интерфейсы с более чем одним методом имеют более одной причины для изменения. 
	- Неопределенность в отношении перемен – это не разрешение на догадки, а указание ОСТАНОВИТЬСЯ и узнать больше. 
- Вы должны различать код, который: защищает от мошенничества и защищает от несчастных случаев

**Валидация:** 
**Используйте интерфейс, когда:**
- Пользователи API должны предоставить подробную информацию о реализации. 
- API имеют несколько реализаций, которые необходимо поддерживать внутри компании. 
- Части API, которые могут измениться, идентифицированы и требуют разделения.

**Не используйте интерфейс:**
- Ради использования интерфейса. 
- Обобщить алгоритм. 
- Когда пользователи могут объявлять свои собственные интерфейсы. 
- Если не понятно, как интерфейс делает код лучше.

## Lesson 6. Error Handling

### Default Error Values

> Обработка ошибок должна быть частью основного кода
> Обработка ошибок в Go "несвязана" decoupled
> При обработке ошибок в Go мы всегда работаем и ИНТЕРФЕЙСОМ error
> Ошибки в Go - это просто значения
> Идея обработки ошибок в GO основана на контексте

**error interface**
```Go
package main

import "fmt"

// http://golang.org/pkg/builtin/#error
type error interface {
	Error() string
}

// http://golang.org/src/pkg/errors/errors.go
type errorString struct {
	s string
}

// http://golang.org/src/pkg/errors/errors.go
func (e *errorString) Error() string {
	return e.s
}

// http://golang.org/src/pkg/errors/errors.go
// New returns an error that formats as the given text.
func New(text string) error {
	return &errorString{text}
}

func main() {
	if err := webCall(); err != nil {
		fmt.Println(err)
		return
	}

	fmt.Println("Life is good")
}

// webCall performs a web operation.
func webCall() error {
	return New("Bad Request")
}
```
[PlayGround](https://go.dev/play/p/beGEdO2QE4g)
![[_resources/error_GO_1.png]]

> **nil** - особое значение в GO 
> Думай о **nil** как о возможности принять люблй тип, те **nil** - это любой тип, который ему нужен
> **ПОМНИ!** Указатели и все ссылочные типы, могут быть **nil**
### Error Variables

> Бывают ситуации, когда вазвращается более 1 ошибки, и пользователь должен понимать - это ошибка "А" или ошибка "В", чтобы принять решение.
> Концепция "переменных" ошибок 

```Go
package main

import (
	"errors"
	"fmt"
)

var (
	// ErrBadRequest is returned when there are problems with the request.
	ErrBadRequest = errors.New("Bad Request")

	// ErrPageMoved is returned when a 301/302 is returned.
	ErrPageMoved = errors.New("Page Moved")
)

func main() {
	if err := webCall(true); err != nil {
		switch err {
		case ErrBadRequest:
			fmt.Println("Bad Request Occurred")
			return

		case ErrPageMoved:
			fmt.Println("The Page moved")
			return

		default:
			fmt.Println(err)
			return
		}
	}

	fmt.Println("Life is good")
}

// webCall performs a web operation.
func webCall(b bool) error {
	if b {
		return ErrBadRequest
	}

	return ErrPageMoved
}

```
[PlayGround](https://go.dev/play/p/JQUJbS20MrE)
### Type as Context (Type как контекст)

```Go
package main

import (
	"fmt"
	"reflect"
)

// An UnmarshalTypeError describes a JSON value that was
// not appropriate for a value of a specific Go type.
type UnmarshalTypeError struct {
	Value string       // description of JSON value
	Type  reflect.Type // type of Go value it could not be assigned to
}

// Error implements the error interface.
func (e *UnmarshalTypeError) Error() string {
	return "json: cannot unmarshal " + e.Value + " into Go value of type " + e.Type.String()
}

// An InvalidUnmarshalError describes an invalid argument passed to Unmarshal.
// (The argument to Unmarshal must be a non-nil pointer.)
type InvalidUnmarshalError struct {
	Type reflect.Type
}

// Error implements the error interface.
func (e *InvalidUnmarshalError) Error() string {
	if e.Type == nil {
		return "json: Unmarshal(nil)"
	}

	if e.Type.Kind() != reflect.Ptr {
		return "json: Unmarshal(non-pointer " + e.Type.String() + ")"
	}
	return "json: Unmarshal(nil " + e.Type.String() + ")"
}

// user is a type for use in the Unmarshal call.
type user struct {
	Name int
}

func main() {
	var u user
	err := Unmarshal([]byte(`{"name":"bill"}`), u) // Run with a value and pointer.
	if err != nil {
		switch e := err.(type) {
		case *UnmarshalTypeError:
			fmt.Printf("UnmarshalTypeError: Value[%s] Type[%v]\n", e.Value, e.Type)
		case *InvalidUnmarshalError:
			fmt.Printf("InvalidUnmarshalError: Type[%v]\n", e.Type)
		default:
			fmt.Println(err)
		}
		return
	}

	fmt.Println("Name:", u.Name)
}

// Unmarshal simulates an unmarshal call that always fails.
func Unmarshal(data []byte, v interface{}) error {
	rv := reflect.ValueOf(v)
	if rv.Kind() != reflect.Ptr || rv.IsNil() {
		return &InvalidUnmarshalError{reflect.TypeOf(v)}
	}

	return &UnmarshalTypeError{"string", reflect.TypeOf(v)}
}
```
[PlayGround](https://go.dev/play/p/BmiblC2Q7MC)

### Behavior as Context (Поведение как контекст)

> "Поведение" как контекст позволяет нам использовать обычный тип в качестве контекста
> **ПОМНИ!** Ошибки - это просто значения 

```Go
package example4

import (
	"bufio"
	"fmt"
	"io"
	"log"
	"net"
)

// client represents a single connection in the room.
type client struct {
	name   string
	reader *bufio.Reader
}

// TypeAsContext shows how to check multiple types of possible custom error
// types that can be returned from the net package.
func (c *client) TypeAsContext() {
	for {
		line, err := c.reader.ReadString('\n')
		if err != nil {
			switch e := err.(type) {
			case *net.OpError:
				if !e.Temporary() {
					log.Println("Temporary: Client leaving chat")
					return
				}

			case *net.AddrError:
				if !e.Temporary() {
					log.Println("Temporary: Client leaving chat")
					return
				}

			case *net.DNSConfigError:
				if !e.Temporary() {
					log.Println("Temporary: Client leaving chat")
					return
				}

			default:
				if err == io.EOF {
					log.Println("EOF: Client leaving chat")
					return
				}

				log.Println("read-routine", err)
			}
		}

		fmt.Println(line)
	}
}

// temporary is declared to test for the existence of the method coming
// from the net package.
type temporary interface {
	Temporary() bool
}

// BehaviorAsContext shows how to check for the behavior of an interface
// that can be returned from the net package.
func (c *client) BehaviorAsContext() {
	for {
		line, err := c.reader.ReadString('\n')
		if err != nil {
			switch e := err.(type) {
			case temporary:
				if !e.Temporary() {
					log.Println("Temporary: Client leaving chat")
					return
				}

			default:
				if err == io.EOF {
					log.Println("EOF: Client leaving chat")
					return
				}

				log.Println("read-routine", err)
			}
		}

		fmt.Println(line)
	}
}
```
[PlayGround](https://go.dev/play/p/sNRSXKtcJKM)

### Find the Bug

> Всегда возвращай интерфейс ошибок (error), вместо пользовательского типа ошибки
> Ты не можешь использовать пользовательский тип ошибок напрямую, ты должен использовать интрефейс error

**Возврат пользовательской ошибки - получаем БАГ**
```Go
package main

import "log"

// customError is just an empty struct.
type customError struct{}

// Error implements the error interface.
func (c *customError) Error() string {
	return "Find the bug."
}

// fail returns nil values for both return types.
// return error
func fail() ([]byte, *customError) {
	return nil, nil
}

func fail() ([]byte, error) {
	nil, nil
}


func main() {
	var err error
	if _, err = fail(); err != nil {
		log.Fatal("Why did this fail?")
	}

	log.Println("No Error")
}
```
[PlayGround](https://go.dev/play/p/CBL-ADH-nSv)

### Wrapping Errors

> Обычно, обработка ошибок связана с логированием
> Мы должны логировать только те вещи, которые делает человек!

**Использование пакета "github.com/pkg/errors" от Dave Chainey**
```Go
package main

import (
	"fmt"

	"github.com/pkg/errors"
)

// AppError represents a custom error type.
type AppError struct {
	State int
}

// Error implements the error interface.
func (c *AppError) Error() string {
	return fmt.Sprintf("App Error, State: %d", c.State)
}

func main() {

	// Make the function call and validate the error.
	if err := firstCall(10); err != nil {

		// Use type as context to determine cause.
		switch v := errors.Cause(err).(type) {
		case *AppError:

			// We got our custom error type.
			fmt.Println("Custom App Error:", v.State)

		default:

			// We did not get any specific error type.
			fmt.Println("Default Error")
		}

		// Display the stack trace for the error.
		fmt.Println("\nStack Trace\n********************************")
		fmt.Printf("%+v\n", err)
		fmt.Println("\nNo Trace\n********************************")
		fmt.Printf("%v\n", err)
	}
}

// firstCall makes a call to a second function and wraps any error.
func firstCall(i int) error {
	if err := secondCall(i); err != nil {
		return errors.Wrapf(err, "firstCall->secondCall(%d)", i)
	}
	return nil
}

// secondCall makes a call to a third function and wraps any error.
func secondCall(i int) error {
	if err := thirdCall(); err != nil {
		return errors.Wrap(err, "secondCall->thirdCall()")
	}
	return nil
}

// thirdCall create an error value we will validate.
func thirdCall() error {
	return &AppError{99}
}
```
[PlayGround](https://go.dev/play/p/Zt1Z5k4HbDG)

## Lesson 7. Packaging

### Language Mechanics

> Пакетно-ориентированный дизайн - часть языка Go. 
> GO - меняет правила игры, здесь нет объектно-ориентированного проектирования

### Design Guidelines (Рекомендации по проектированию)

> В других языках мы используем папки для исходного кода, мы организуем систему хранения в этих папках и это работало, НО мы не можем это делать в GO.
> С точки зрения языка GO, каждая папка становиться статической библиотекой.
> GO скомпилирует каждую папку в собственную статическую библиотеку, которая действительно независима от любой другой статической библиотеки.
> Это как микросервисная архитектура в исходном дереве проекта, где каждая папка - это действительно независимый компнонент. И есть связь между этиим компонентами.
> В GO не существует понятия подпакета!

> Два пакета не могут импортировать друг друга. 
> Импорт - улица с односторонним движением!

**Три пункта философии Пакетно-ориентированного дизайна**
4. Пакет должен быть целенаправленный. Пакет должен обеспечивать, а не содержать.
	- Название пакета описывает, что он предоставляет. 
	  Хорошее название - http, json, io. 
	  Плохое название - utils, common, headers.
	- Пакеты не должны превращаться в свалку разрозненных задач. Пакет должен быть разработан в качестве решения одной задачи
5. Удобство использования
	- Пакеты должны быть интуитивно понятными и простыми в использовании.
	- Пакеты должны учитывать их влияние на ресурсы и производительность.
	- Пакеты должны защищать приложение пользователя от каскадных изменений.
	- Пакеты должны сокращать, минимизировать и упрощать свою кодовую базу.
6. Чтобы пакеты были портативными, они должны быть разработаны с учетом возможности повторного использования.
	- Пакеты должны стремиться к высочайшему уровню переносимости.
	- Пакеты должны сокращать политику настройки, когда это разумно и практично.
	- Пакеты не должны становиться единой точкой зависимости.

### Package-Oriented Design

**Пакетно-ориентированный дизайн состоит из трёх частей**
7. Позволяет разработчику определить, где находится пакет внутри проекта GO.
8. Позволяет определять, что это за проект GO и как структурирован этот проект.
9. Улучшает общение между членами команды и способствует чистому дизайну пакетов и архитектуре проекта, которую можно обсуждать.

>Пакетно-ориентированный дизайн не основан на одной структуре проекта
>Два типа проектов: Kit Projects, Application Projects
>**Kit Project** - стандартная библиотека для вашей компании или человека 
>**Application Projects** - прикладной проект

**Project structure**
**Kit                     Application**
├── CONTRIBUTORS        ├── cmd/
├── LICENSE             ├── internal/
├── README.md           │   └── platform/
├── cfg/                └── vendor/
├── examples/
├── log/
├── pool/
├── tcp/
├── timezone/
├── udp/
└── web/

**Kit**
> Пакеты на одном уровне не должы импортировать друг друга

**Application**
├── cmd/
|   ├── servi/
|   |   ├── cmdupdate/
|   |   ├── cmdquery/
|   |   └── servi.go
|   └── servid/
|       ├── handlers/
|       ├── routes/
|       ├── tests/
|       └── servid.go
├── internal/
│   ├── attachments/
|   ├── locations/
|   ├── orders/
|   |   ├── customers/
|   |   ├── items/
|   |   ├── tags/
|   |   └── orders.go
|   ├── registrations/
|   └── platform/
|       ├── crypto/
|       ├── mongo/
|       └── json/
└── vendor/

> **cmd/** - здесь живут двоичные файлы
> **servid** - с "d" на конце, означает что это демон
> **internal/** - особенная папка. Одним из преимуществ использования имени **internal/** является то, что проект получает дополнительный уровень защиты от компилятора. Ни один пакет за пределами этого проекта не может импортировать пакеты из **internal/**. Таким образом, эти пакеты являются внутренними только для этого проекта.
> **platform/** - пакеты, являющиеся базовыми, но специфичными для проекта, обеспечивающие поддержку таких вещей, как базы данных, аутентификация или даже маршалинг.
> **vendor/** - здесь храняться все сторонние пакеты, включая пакеты **Kit**


## Lesson 8. Goroutines.

### Go Scheduler Internals (Внутреннее устройство планировщика Go)

> Когда запускается программа на GO, проверяется сколько ядер доступно

### Language Mechanics 

**Goroutines and concurrency**
```Go
package main

import (
	"fmt"
	"runtime"
	"sync"
)

func init() {

	// Allocate one logical processor for the scheduler to use.
	runtime.GOMAXPROCS(1)
}

func main() {

	// wg is used to manage concurrency.
	var wg sync.WaitGroup
	wg.Add(2)

	fmt.Println("Start Goroutines")

	// Create a goroutine from the lowercase function.
	go func() {
		lowercase()
		wg.Done()
	}()

	// Create a goroutine from the uppercase function.
	go func() {
		uppercase()
		wg.Done()
	}()

	// Wait for the goroutines to finish.
	fmt.Println("Waiting To Finish")
	wg.Wait()

	fmt.Println("\nTerminating Program")
}

// lowercase displays the set of lowercase letters three times.
func lowercase() {

	// Display the alphabet three times
	for count := 0; count < 3; count++ {
		for r := 'a'; r <= 'z'; r++ {
			fmt.Printf("%c ", r)
		}
	}
}

// uppercase displays the set of uppercase letters three times.
func uppercase() {

	// Display the alphabet three times
	for count := 0; count < 3; count++ {
		for r := 'A'; r <= 'Z'; r++ {
			fmt.Printf("%c ", r)
		}
	}
}
```
[PlayGround](https://go.dev/play/p/4n6G3uRDc83)

**Goroutine time slicing**
```Go
package main

import (
	"crypto/sha1"
	"fmt"
	"runtime"
	"strconv"
	"sync"
)

func init() {

	// Allocate one logical processor for the scheduler to use.
	runtime.GOMAXPROCS(1)
}

func main() {

	// wg is used to manage concurrency.
	var wg sync.WaitGroup
	wg.Add(2)

	fmt.Println("Create Goroutines")

	// Create the first goroutine and manage its lifecycle here.
	go func() {
		printHashes("A")
		wg.Done()
	}()

	// Create the second goroutine and manage its lifecycle here.
	go func() {
		printHashes("B")
		wg.Done()
	}()

	// Wait for the goroutines to finish.
	fmt.Println("Waiting To Finish")
	wg.Wait()

	fmt.Println("Terminating Program")
}

// printHashes calculates the sha1 hash for a range of
// numbers and prints each in hex encoding.
func printHashes(prefix string) {

	// print each has from 1 to 10. Change this to 50000 and
	// see how the scheduler behaves.
	for i := 1; i <= 10; i++ {

		// Convert i to a string.
		num := strconv.Itoa(i)

		// Calculate hash for string num.
		sum := sha1.Sum([]byte(num))

		// Print prefix: 5-digit-number: hex encoded hash
		fmt.Printf("%s: %05d: %x\n", prefix, i, sum)
	}

	fmt.Println("Completed", prefix)
}
```
[PlayGround](https://go.dev/play/p/QtNVo1nb4uQ)

**Goroutines and parallelism**
```Go
package main

import (
	"fmt"
	"runtime"
	"sync"
)

func init() {

	// Allocate two logical processors for the scheduler to use.
	runtime.GOMAXPROCS(2)
}

func main() {

	// wg is used to wait for the program to finish.
	// Add a count of two, one for each goroutine.
	var wg sync.WaitGroup
	wg.Add(2)

	fmt.Println("Start Goroutines")

	// Declare an anonymous function and create a goroutine.
	go func() {

		// Display the alphabet three times.
		for count := 0; count < 3; count++ {
			for r := 'a'; r <= 'z'; r++ {
				fmt.Printf("%c ", r)
			}
		}

		// Tell main we are done.
		wg.Done()
	}()

	// Declare an anonymous function and create a goroutine.
	go func() {

		// Display the alphabet three times.
		for count := 0; count < 3; count++ {
			for r := 'A'; r <= 'Z'; r++ {
				fmt.Printf("%c ", r)
			}
		}

		// Tell main we are done.
		wg.Done()
	}()

	// Wait for the goroutines to finish.
	fmt.Println("Waiting To Finish")
	wg.Wait()

	fmt.Println("\nTerminating Program")
}
```
[PlayGround](https://go.dev/play/p/ybZ84UcLW81)

## Lesson 9. Data Races.

> **Data Races** — это когда две или более горутины пытаются читать и писать в один и тот же ресурс одновременно. Условия гонки могут создавать ошибки, которые кажутся совершенно случайными или никогда не проявляются, поскольку они повреждают данные. Атомарные функции и мьютексы — это способ синхронизации доступа к общим ресурсам между горутинами.

- Горутины должны быть скоординированы и синхронизированы. 
- Когда две или более горутины пытаются получить доступ к одному и тому же ресурсу, возникает гонка данных. 
- Атомарные функции и мьютексы могут обеспечить необходимую нам поддержку.
### Race Detection

> В многопоточности должна быть координация
> 1. Либо ты синхронизируешь доступ к общему состоянию
> 2. Либо ты должен координировать горутины, чтобы они были предсказуемы
> Используй Атомарные функции, мьютексы или каналы

> Атомарные инструкции (функции) - самый быстрый путь, на уровне оборудования (железа)
> Мьютекс - следующие по быстроте
> Каналы - самые медленные (сложная логика реализации)

**DATA RACE EXAMPLE**
```Go
package main

import (
	"fmt"
	"sync"
)

// counter is a variable incremented by all goroutines.
var counter int

func main() {

	// Number of goroutines to use.
	const grs = 2

	// wg is used to manage concurrency.
	var wg sync.WaitGroup
	wg.Add(grs)

	// Create two goroutines.
	for g := 0; g < grs; g++ {
		go func() {
			for i := 0; i < 2; i++ {

				// Capture the value of Counter.
				value := counter

				// Increment our local value of Counter.
				value++

				// Store the value back into Counter.
				counter = value
			}

			wg.Done()
		}()
	}

	// Wait for the goroutines to finish.
	wg.Wait()
	fmt.Println("Final Counter:", counter)
}
```
[PlayGround](https://go.dev/play/p/czqXM5wOspX)

> В GO есть детектор гонки!
> `$ go build -race`
> `$ ./example1`
> Трассировку получаем только после запуска скомпилируемой программы


### Atomic Functions

>Атомарные функци работают со значениями от 4 - 8 байт
 Атомарные функции требуют от нас точности, те не просто **int**, а **int64**, **int32**

```Go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

// counter is a variable incremented by all goroutines.
var counter int64

func main() {

	// Number of goroutines to use.
	const grs = 2

	// wg is used to manage concurrency.
	var wg sync.WaitGroup
	wg.Add(grs)

	// Create two goroutines.
	for g := 0; g < grs; g++ {
		go func() {
			for i := 0; i < 2; i++ {
				atomic.AddInt64(&counter, 1)
			}

			wg.Done()
		}()
	}

	// Wait for the goroutines to finish.
	wg.Wait()

	// Display the final value.
	fmt.Println("Final Counter:", counter)
}
```
[PlayGround](https://go.dev/play/p/5ZtLaX7zxt7)

### Mutexes

**MUTEX EXAMPLE CODE**
```Go
package main

import (
	"fmt"
	"sync"
)

// counter is a variable incremented by all goroutines.
var counter int

// mutex is used to define a critical section of code.
var mutex sync.Mutex

func main() {

	// Number of goroutines to use.
	const grs = 2

	// wg is used to manage concurrency.
	var wg sync.WaitGroup
	wg.Add(grs)

	// Create two goroutines.
	for g := 0; g < grs; g++ {
		go func() {
			for i := 0; i < 2; i++ {

				// Only allow one goroutine through this critical section at a time.
				mutex.Lock()
				{
					// Capture the value of counter.
					value := counter

					// Increment our local value of counter.
					value++

					// Store the value back into counter.
					counter = value
				}
				mutex.Unlock()
				// Release the lock and allow any waiting goroutine through.
			}

			wg.Done()
		}()
	}

	// Wait for the goroutines to finish.
	wg.Wait()
	fmt.Printf("Final Counter: %d\n", counter)
}
```
[PlayGround](https://go.dev/play/p/ZKE2v9H4oS-)

**READ WRITE MUTEX EXAMPLE**
```Go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"sync/atomic"
	"time"
)

// data is a slice that will be shared.
var data []string

// rwMutex is used to define a critical section of code.
var rwMutex sync.RWMutex

// Number of reads occurring at ay given time.
var readCount int64

// init is called prior to main.
func init() {
	rand.Seed(time.Now().UnixNano())
}

func main() {

	// wg is used to manage concurrency.
	var wg sync.WaitGroup
	wg.Add(1)

	// Create a writer goroutine.
	go func() {
		for i := 0; i < 10; i++ {
			writer(i)
		}
		wg.Done()
	}()

	// Create eight reader goroutines.
	for i := 0; i < 8; i++ {
		go func(id int) {
			for {
				reader(id)
			}
		}(i)
	}

	// Wait for the write goroutine to finish.
	wg.Wait()
	fmt.Println("Program Complete")
}

// writer adds a new string to the slice in random intervals.
func writer(i int) {

	// Only allow one goroutine to read/write to the slice at a time.
	rwMutex.Lock()
	{
		// Capture the current read count.
		// Keep this safe though we can due without this call.
		rc := atomic.LoadInt64(&readCount)

		// Perform some work since we have a full lock.
		time.Sleep(time.Duration(rand.Intn(100)) * time.Millisecond)
		fmt.Printf("****> : Performing Write : RCount[%d]\n", rc)
		data = append(data, fmt.Sprintf("String: %d", i))
	}
	rwMutex.Unlock()
	// Release the lock.
}

// reader wakes up and iterates over the data slice.
func reader(id int) {

	// Any goroutine can read when no write operation is taking place.
	rwMutex.RLock()
	{
		// Increment the read count value by 1.
		rc := atomic.AddInt64(&readCount, 1)

		// Perform some read work and display values.
		time.Sleep(time.Duration(rand.Intn(10)) * time.Millisecond)
		fmt.Printf("%d : Performing Read : Length[%d] RCount[%d]\n", id, len(data), rc)

		// Decrement the read count value by 1.
		atomic.AddInt64(&readCount, -1)
	}
	rwMutex.RUnlock()
	// Release the read lock.
}
```


## Lesson 10. Channels.

> Каналы - ключевая особенность языка GO
> Каналы - это инструмент, помогает скоординировать горутины и организовать раб. процесс

> Каналы позволяют горутинам взаимодействовать друг с другом посредством использования семантики сигнализации. Каналы осуществляют эту передачу сигналов посредством отправки/получения данных или путем определения изменений состояния отдельных каналов. Не проектируйте программное обеспечение с идеей, что каналы представляют собой очередь, сосредоточьтесь на сигнализации и семантике, которые упрощают необходимую оркестровку.

### Language Mechanics

> Каналы бывают буферезированные и небуферезированные
> Unbuffer channel - работает медленне, поскольку гарантирует получение сигнала (ждёт ответ)
> Buffer channel - работает быстрее и непрерывнее, но не гарантирует получение сигнала
> Сигналы в канал бывают с данными либо без данных


**SIMPLE UNBUFFER CHANNEL EXAMPLE**
```Go
func basicSendRecv() {
	ch := make(chan string)
	go func() {
		ch <- "hello"
	}()
		fmt.Println(<-ch)
}
```

**SIMPLE UNBUFFER CHANNEL without data**
```Go
func signalClose() {
	ch := make(chan struct{})
	go func() {
		time.Sleep(100 * time.Millisecond)
		fmt.Println("signal event")
		close(ch)
	}()
	<-ch
	fmt.Println("event recived")
}
```

**Наглядный пример работы небуферезированного канала**
_reciver wait sender_
_пока канал не получит данные рутина заблочена_
```Go
func signalAck() {
	ch := make(chan string)
	go func() {
		fmt.Println("1", <-ch)
		ch <- "ok done"
	}()
	ch <- "do this"
	fmt.Println("2", <-ch)

	// 1 do this
	// 2 ok done
}
```

**Buffered channel with range**
```Go
func closeRange() {
	ch := make(chan int, 5)
	for i := 0; i < 5; i++ {
		ch <- i
	}
	close(ch)

	for v := range ch {
		fmt.Println(v)
	}
}
```

### Channel Design Guidelines

10. Используйте каналы для оркестровки и координации горутин. 
	- Сосредоточьтесь на семантике сигналов, а не на обмене данными. 
	- Сигнализация с данными или без данных. 
	- Под вопросом их использование для синхронизации доступа к общему состоянию. 
		- Бывают случаи, когда каналы могут быть проще для этого, но изначально вопрос.
11. Небуферизованные каналы: 
	- Получение происходит перед отправкой. 
	- Преимущество: 100% гарантия получения отправленного сигнала. 
	- Стоимость: неизвестная задержка при получении сигнала.
12. Буферизованные каналы: 
	- Отправка происходит до получения. 
	- Преимущество: Уменьшите задержку блокировки между сигналами. 
	- Стоимость: нет гарантии, что отправленный сигнал будет получен. 
		- Чем больше буфер, тем меньше гарантия. 
		- Буфер со значением 1 может дать вам одну отложенную отправку гарантии.
13. Закрытие каналов: 
	- Закрытие происходит до получения. (например, буферизованный) 
	- Сигнализация без данных. 
	- Идеально подходит для сигнализации об отмене и сроках.
14. NIL каналы: 
	- Блок отправки и получения. 
	- Отключить сигнализацию.
	- Идеально подходит для ограничения скорости или кратковременных остановок.

### Concurrent Software Design

## Lesson 11. Concurrency Patterns.

> Существует множество различных шаблонов, которые мы можем создать с помощью горутин и каналов. Два интересных шаблона — это объединение ресурсов и одновременный поиск

### Context

> Context - пакет для работы, отмены горутин
> Context предоставляет такие инструменты работы с потоакми как timout, cancel etc..
> Используй контекст по минимуму, всегда лучше передать значение вниз по стеку вызовов или поделиться им как параметром в функциях или методах.
> Каждый раз, когда используешь контекст - ПОДУМАЙ!, можно ли обойтисб без него.

**Context with value**
```Go
package main

type user struct {
	name string
}

type userKey int 

func main() {
	u := user {
		name: "Bill",
	}

	const uk userKey = 0

	ctx := context.WithValue(context.Background(), uk, &u)
	if u, ok := ctx.Value(uk).(*user); ok {
		fmt.Println("User", u.name)
	}

	if _, ok := ctx.Value(0).(*user); !ok {
		fmt.Println("User not found")
	}
}
```

> Всегда, когда работаем с контекстом, нужно использовать родительский контекст `context.Background()`

> Если собираешься хранить значения в нутри контекста не используй встроенные типы, переопредели свой тип (например объяви свой собственный тип ключа) как в примере выше.

**Context with cancel**
```Go
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {

	// Create a context that is cancellable only manually.
	// The cancel function must be called regardless of the outcome.
	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	// Ask the goroutine to do some work for us.
	go func() {

		// Wait for the work to finish. If it takes too long move on.
		select {
		case <-time.After(100 * time.Millisecond):
			fmt.Println("moving on")

		case <-ctx.Done():
			fmt.Println("work complete")
		}
	}()

	// Simulate work.
	time.Sleep(50 * time.Millisecond)

	// Report the work is done.
	cancel()

	// Just hold the program to see the output.
	time.Sleep(time.Second)
}
```

**Context with deadline**
```Go
package main

import (
	"context"
	"fmt"
	"time"
)

type data struct {
	UserID string
}

func main() {

	// Set a deadline.
	deadline := time.Now().Add(150 * time.Millisecond)

	// Create a context that is both manually cancellable and will signal
	// a cancel at the specified date/time.
	ctx, cancel := context.WithDeadline(context.Background(), deadline)
	defer cancel()

	// Create a channel to received a signal that work is done.
	ch := make(chan data, 1)

	// Ask the goroutine to do some work for us.
	go func() {

		// Simulate work.
		time.Sleep(200 * time.Millisecond)

		// Report the work is done.
		ch <- data{"123"}
	}()

	// Wait for the work to finish. If it takes too long move on.
	select {
	case d := <-ch:
		fmt.Println("work complete", d)

	case <-ctx.Done():
		fmt.Println("work cancelled")
	}
}
```

**Context with time out**
```Go
package main

import (
	"context"
	"fmt"
	"time"
)

type data struct {
	UserID string
}

func main() {

	// Set a deadline.
	deadline := time.Now().Add(150 * time.Millisecond)

	// Create a context that is both manually cancellable and will signal
	// a cancel at the specified date/time.
	ctx, cancel := context.WithDeadline(context.Background(), deadline)
	defer cancel()

	// Create a channel to received a signal that work is done.
	ch := make(chan data, 1)

	// Ask the goroutine to do some work for us.
	go func() {

		// Simulate work.
		time.Sleep(200 * time.Millisecond)

		// Report the work is done.
		ch <- data{"123"}
	}()

	// Wait for the work to finish. If it takes too long move on.
	select {
	case d := <-ch:
		fmt.Println("work complete", d)

	case <-ctx.Done():
		fmt.Println("work cancelled")
	}
}
```

**Context time out with request**
```Go
package main

import (
	"context"
	"io"
	"log"
	"net/http"
	"os"
	"time"
)

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 50*time.Second)
	defer cancel()

	req, err := http.NewRequest("GET", "https://www.ya.ru/index.html", nil)
	if err != nil {
		log.Println(err)
		return
	}

	var tr http.Transport
	client := http.Client{
		Transport: &tr,
	}

	ch := make(chan error, 1)
	go func() {
		log.Println("starting request")

		resp, err := client.Do(req)
		if err != nil {
			ch <- err
			return
		}

		defer resp.Body.Close()

		io.Copy(os.Stdout, resp.Body)
		ch <- nil

	}()

	select {
	case <-ctx.Done():
		log.Println("timeout, cancel work...")
		tr.CancelRequest(req)
		log.Println(<-ch)
	case err := <-ch:
		if err != nil {
			log.Println(err)
		}
	}

}
```

## Lesson 12. Testing.







## Форматированный вывод

Каждый спецификатор представляет определенный тип данных:

- `%t`: для вывода значений типа boolean (true или false)
    
- `%b`: для вывода целых чисел в двоичной системе
    
- `%c`: для вывода символов, представленных числовым кодом
    
- `%d`: для вывода целых чисел в десятичной системе
    
- `%o`: для вывода целых чисел в восьмеричной системе
    
- `%q`: для вывода символов в одинарных кавычках
    
- `%x`: для вывода целых чисел в шестнадцатеричной системе, буквенные символы числа имеют нижний регистр a-f
    
- `%X`: для вывода целых чисел в шестнадцатеричной системе, буквенные символы числа имеют верхний регистр A-F
    
- `%U`: для вывода символов в формате кодов Unicode, например, U+1234
    
- `%e`: для вывода чисел с плавающей точкой в экспоненциальном представлении, например, -1.234456e+78
    
- `%E`: тоже самое что `%e` но в верхнем регистре, например, -1.234456E+78
    
- `%f`: для вывода чисел с плавающей точкой, например, 123.456
    
- `%F`: то же самое, что и %f
    
- `%g`   %e для огромных экспонент, %f в противном случае
    
- `%G`    %E для огромных экспонент, %F в противном случае
    
- `%s`: для вывода строки
    
- `%p`: для вывода значения указателя - адреса в шестнадцатеричном представлении (указатели мы пройдем на следующих уроках)
    
- `%T` для вывода типа переменной

Также можно применять универсальный спецификатор `%v`, который для типа boolean аналогичен 
`%t`, для целочисленных типов - `%d`, для чисел с плавающей точкой - `%g`, для строк - `%s`.

К спецификаторам можно добавлять различные флаги, которые влияют на форматирование значений. Например, число перед спецификатором указывает, какую минимальную длину в символах будет занимать выводимое значение. Например, `%9f` - число с плавающей точкой будет занимать как минимум 9 позиций. Если ширина больше, чем требуется значению, то заполняется пробелами.

Для чисел с плавающей точкой можно указать точность или количество символов в дробной части. Для этого количество символов указывается после точки: `%.2f` - две цифры в дробной части после точки. Например, варианты форматирования чисел с плавающей точкой:

- `%f`: точность и ширина значения по умолчанию
    
- `%9f`: ширина - 9 символов и точность по умолчанию  
    (число с плавающей точкой будет занимать как минимум 9 позиций. Если ширина больше, чем требуется значению, то заполняется пробелами.)
    
- `%.2f`: ширина по умолчанию и точность - 2 символа
    
- `%9.2f`: ширина - 9 и точность - 2
    
- `%9.f`: ширина - 9 и точность - 0

Также из флагов следует отметить дефис -, который дополняет значение пробелами не слева, как по умолчанию, а справа.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
- [Ultimate GO Githuub](https://github.com/ardanlabs/gotraining/tree/master/topics/go)
