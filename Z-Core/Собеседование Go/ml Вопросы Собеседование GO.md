2025022415:34
___
Date: 24-02-2025 | 15:34
Tags: #go #собеседование #вопросы
mapofcontents: [[zero-links|OO Links]]
___
## GO - Собеседование

### Вопросы

- [[00 Отличие Go от других ЯП|Отличия GO от других языков]]
- [[00 Типизация|Расскажи про типизацию - статическая и динамическая типизация, строгая и слабая.]]
- [[00 Встроенные типы в Go|Какие есть встроенные типы в Go?]]
- [[00 Отличие Map от Slice|Чем мапа отличается от слайса?]]
- [[00 Отличие Array от Slice|Чем отличается массив и слайс?]]
- [[00 Как работает Slice|Как работает слайс? В чем отличие Array от Slice?]]
- [[00 Переполнение int|Что будет при переполнении int?]]
- [[00 Что такое defer|Что такое defer?]]
- как работает связка panic recovery defer?
- [[00 Что такое map?|Что такое map?]]
- [[00 Что такое Slice|Что такое слайс?]]
- [[00 Реализация Set|Как в Go реализовать Set?]]
- [[00 Строки в Go|Как устроены строки в Go?]]
- [[00 Посчитать символы в строке|Как посчитать символы в строке?]]
- [[00 Сортировка массива по частоте|Как отсортировать массив по частоте встречаемости элементов?]]
- [[00 Сложить две строки в Go|Что будет если сложить две строки?]]
- [[00 Сложить две строки в Go|Как эффективно конкантенировать строки?]]
- [[00 Структуры данных|Какие знаешь структуры данных?]]
- [[00 Структуры данных|Что такое структуры данных в Go? Какие знаешь?]]
- [[00 Устройство Map в Go|Расскажи что такое мапа в Go? Кау устроена?]]
- [[00 Многопоточная работа с Map|Многопоточная работа с мапами. Как работать, какие есть проблемы и нюансы?]]
- [[00 Select в Go|Зачем нужен select в go?]]
- [[00 Context в Go|Зачем нужен context?]]
- [[00 Error в Go|Что такое error в Go и какие бывают?]]
- [[00 error group в Go|Работал ли с ErrorGroup?]]
- [[00 Что такое Interface|Что такое интерфейсы?]]
- [[ml Вопросы Собеседование GO|Что такое пустой интерфейс?]]
- [[00 NIL Interface|Когда интерфейс равен nil?]]
- [[00 Как работают интерфейсы в Go|Как работают интерфейсы в go?]]
- [[00 Как работают интерфейсы в Go|Чем представлен интерфейс в Go?]]
- [[00 Где хранить интерфейсы|Где хранить интерфейсы?]]
- [[00 http методы|Какие http методы бывают?]]
- В чем отличия http/1 от http/2?
- что знаешь, слышал про http/3
- [[00 Что такое ACID|Поясни что такое ACID?]]
- [[00 Что такое CQRS|Расскажи что такое CQRS?]]
- [[00 Запись и чтение в CQRS|Запись и чтение в CQRS]]
- [[00 Нормализация и денормализация БД|Нормализация и денормализация БД]]
- [[00 Reflection в Go|Что такое рефлексия и зачем она нужна?]]
- [[00 Репликация в базах данных|Зачем нужна репликация в базах данных?]]
- [[00 Синхронная и асинхронная репликация|Расскажи про синхронную и асинхронную репликацию в базах данных.]]
- [[00 Batching updates|Как забатчить кучу апдейтов в таблице?]]
- [[00 Индексы в базах данных|Зачем нужны индексы в базах данных?]]
- [[00 Индексы в BD - отрицательные эффекты|Какие есть отрицательные эффекты от индексов?]]
- [[00 Триггеры в BD|Когда использовать триггеры в бд?]]
- [[00 Тормозит запрос к базе. Explain и Explain Analyze|Что делать если тормозит запрос к базе? Explain и Explain Analyze, отличия.]]
- [[00 NOTIFY в postgreSQL|Что такое notify в postgresql?]]
- [[00 Составной индекс в BD|Есть составной индекс на поля юзера - first name и last name. Делаем запрос на last name. Сработает ли индекс?]] // нет
- [[00 Составной индекс в BD|Как работает составной индекс?]]
- Какие бывают индексы в БД
- Чем отличается B-Tree индекс от Hash индекса
- [[00 Тормозит запрос к базе. Explain и Explain Analyze|Как диагностировать почему база медленная?]]
- Эффективно ли индексировать булевые поля в базе? // не эффективно нзкая селективность
- [[00 Вложенные транзакции в BD|Работал ли со вложенными транзакциями?]]
- [[00 Блокировки в BD|Расскажи про блокировки в базах данных.]]
- [[00 Уровни изоляции в базах данных|Уровни изоляции в базах данных]]
- [[00 GIN индекс в PostgreSQL|Что такое gin индекс?]]
- [[00 Читаем файл с конца|Как прочитать большой файл с конца?]]
- [[00 Конурентность vs Параллелелизм|Чем отличается конкурентность от параллелелизма?]]
- Модель гошной конкуренции Fork/Join
- [[00 Goroutines in Go|Что такое горутины? Как используются?]]
- [[00 Создание Gorutine|О чем ты думаешь когда создаешь горутину?]]
- [[00 Запуск 100к горутин|Почему на Go можно запустить 100к горутин?]]
- [[00 Примитивы синхронизации в Go|Расскажи про примитивы синхронизации]]
- [[00 Sync package|Что есть в пакете sync и для чего он нужен?]]
- [[00 Пакеты sync.Pool sync.Cond|Работал ли с sync.Pool? Что это такое?]]
- [[00 Пакет sync.Pool - подробно|Пакет sync.Pool подробно]]
- [[00 Data Race и Race Condition в Go|Что такое data race и race condition?]]
- [[00 Переключение контекста между горутинами|Как переключается контекст между горутинами?]]
- [[00 Конвеер (pipeline) из gorutine|Как построить конвеер из горутин?]]
- [[00 Переключение горутин|Когда горутина может быть переключена на другую?]]
- [[00 Почему может зависнуть горутина|Почему может зависнуть горутина?]]
- [[00 Как между собой общаются горутины|Как между собой общаются горутины?]]
- Какие сложности могут возникать при конкурентной обработке данных?
- [[00 Несколько мьютексов в одной функции|Можно ли использовать несколько мьютексов в одной функции?]]
- [[00 Мьютексы и порядок вызова горутин|Гарантируют ли мьютексы какой-то порядок вызова горутин? Например если три горутины ожидают анлока мьютекса - определен ли порядок какая выполнится?]]
- Каналы
- [[00 Каналы. Аксиомы каналов|Расскажи про каналы. Что такое аксиомы каналов?]]
- [[00 Виды каналов. Закрытие каналов|Какие бывают каналы? Кто должен закрывать канал?]]
- [[00 Закрытые каналы. Использование|Расскажи тонкости использования закрытых каналов.]]
- [[00 Паттерны fan-in fan-out|Паттерны fan-in fan-out]]
- [[00 Fan-in, Fan-out простое объяснение|Fan-in Fan-out]]
- [[00 Потокобезопасная очередь|Потокобезопасная очередь]]
- [[00 Паттерн Worker Pool. Реализация.|Реализация паттерна worker pool]]
- [[00 Паттерн "пул горутин"|Паттерн пулл горутин]]
- [[00 Ассинхронный пайплайн|Ассинхронный пайплайн]]
- Утечка горутин
- Мьютексы на каналах
- Sync fans
- Многозадачность. Работа планировщика. Mодель GMP. Контекст переключения горутин. Глобальная, локальная очередь. Статусы горутин. IO bound, CPU bound. 
- [[00 Ретрансляция из канала|Как ретранслировать сообщение из канала в пять разных мест приложения?]]
- [[00 Патерн Publish-Subscribe|Патерн Publish-Subscribe]]
- [[00 Отказ от каналов|Приходилось ли отказываться от каналов?]]
- [[00 JSONB|Что такое jsonb?]]
- [[00 Поиск по JSONB|Можно ли эффективно искать по jsonb?]]
- [[00 Паттерн Saga|Расскажи про паттерн сага и их реализации?]]
- [[00 Протокол работы gRPC|На каком протоколе работает grpc?]]
- [[00 Плюсы и минусы gRPC|Плюсы и минусы grpc]]
- [[00 Обратная совместимость с gRPC|Как работает обратная совместимость с grpc?]]
- [[00 Меняем контракт gRPC|Что можно поменять в контракте grpc чтобы ничего не сломалось?]]
- [[00 Мета данные в gRPC|Что такое мета данные в grpc?]]
- [[00 Экономия данных с gRPC|В чем экономия данных с grpc?]]
- Есть ли опыт работы с kubernetes?
- Расскажи про свой опыт с kubernetes.
- [[00 Что такое Redis|Что такое Redis?]]
- [[00 Команды Redis|Что использовал в redis? Какие команды редиса знаешь?]]
- Что такое pub/sub в контексте Kafka, Redis
- [[00 Использование Кэша|Когда кеш использовать плохо?]]
- [[00 pprof в Go|Что делает pprof?]]
- [[00 Опыт работы с pprof|Какой есть опыт работы с pprof?]]
- [[00 Stop-the-World|Как долго происходит stop the world?]]
- [[00 Graceful degradation|Что такое graceful degradation?]]
- [[00 Graceful shutdown|Как реализовать graceful shutdown?]]
- [[00 Пакеты для тестирования|Какие пакеты использовал для тестирования?]]
- [[00 Наследование vs Встраивание|Опыт с языками с наследованием, в чем разница между наследованием и встраиванием?]]
- [[00 SOLID|Расскажи про SOLID]]
- Можно ли на Go писать соблюдая принципы SOLID? // да
- [[00 Принципы и подходы к проектированию|Какими еще принципами пользуешься? KISS]]
- [[00 Паттерн Anti-Corruption Layer (ACL)|Что такое паттерн anti corruption layer?]]
- [[00 Inversion of Control (IoC)|Что такое Inversion of control?]]
- [[00 garbage collector|Как включается garbage collector?]]
- По какому принципу работает garbage collector?
- Профилирование, pprof
- [[00 Микросервисы|Расскажи про микросервисы. В чем их плюсы и минусы, Что нужно помнить при создании микросервиса?]]
- [[00 High cohesion, low coupling в микросервисах|High cohesion / low coupling в микросервисах. Что это такое, зачем нужно?]]
- [[00 Переход от Монолита к Микросервисам|Как уходить из монолита к микросервисам?]]
- [[00 Коммуникации в микросервисах|Какие есть способы коммуникации в микросервисах?]]
- [[00 stateful и stateless в микросервисах|Чем отличается stateful и stateless в разработке микросервисов?]]
- [[00 Масштабирование stateless и stateful|Какие микросервисы легде масштабировать - stateless или stateful? Почему?]]
- [[00 Event-Driven подход и Apache Kafka|Как делал микросервисы на последнем месте работы?]]
- [[00 Архитектура для микросервисов|Какую архитектуру использовал для микросервисов?]]
- [[00 Паттерны микросервисов|Какие паттерны микросервисов знаешь?]]
- [[00 Client to the same instance. Microservices|Как масштабировать систему где один клиент должен попадать на тот же инстанс?]]
- [[00 Ограничиваем ручку API|Как ограничить ручку API 10 тысяч раз в минуту?]]
- [[00 Chain of Responsibility|Что такое Chain of responsibility?]]
- Какие есть способы обхода деревьев?
- За сколько итераций можно найти элемент в бинарном дереве из 1000000 элементов
- Сложность быстрой сортировки
- За сколько можно найти элемент в хэшмапе
- [[00 Kafka и RabbitMQ|Что такое Apache Kafka? Зачем нужно? В чем отличия?]]
- [[00 least once, amost once и exactly once|least once, at most once и exactly once]]
- [[00 Kafka 100к сообщений в секунду|Как в кафку загнать 100к сообщений в секунду?]]
- [[00 Партиции (partitions) в Apache Kafka|Зачем нужны партишены в кафке?]]
- [[00 least once, amost once и exactly once|Какие есть гарантии доставки в kafka?]]
- [[00 Что такое consumer lag в Kafka|Что такое consumer lag в kafka?]]
- [[00 Observability (graphana, prometheus)|Что использовал для observability? (graphana, prometheus)]]
- [[00 Ключи идемпотентности в микросервисах|Для чего могут использоваться ключи идемпотентности?]]
- [[00 Несколько микросервисов - одна БД|Как сделать чтобы несколько микросервисов использовали оптимально одну базу?]]
- Что будет если выкатили баг в прод?
- [[00 Согласованный деплой. Виды деплоя|Что такое согласованный деплой? Какие виды деплоя знаешь?]]
- Что такое динамическое объявление переменной в Golang?
- Что такое указатели Golang?  
- Перечислите операторы языка программирования Go.
- Расскажите об ООП в Golang.
- Что такое FMT Golang?
- Опишите этапы тестирования с помощью Golang.  
- Что такое Goroutines (Горутины)?  
- Что такое GOPATH и GOROOT?  
- Что такое интерфейсы Go?  
- Что такое L-value и R-value в Golang?  
- Что такое рабочее пространство Go?  
- Что такое затенение?  
- Какова цель переменной среды GOPATH?  
- Как используются указатели в Go?  
- Какие типы указателей есть у Go?
- Есть ли у Go исключения? Как Go обрабатывает ошибки?  
- Когда бы вы использовали оператор break в Go?  
- Как нетипизированные константы взаимодействуют с системой набора текста Golang?  
- В чем разница между = и := в Go?  
- Поддерживает ли Go перегрузку метода?  
- Что делает Go таким быстрым?  
- Как реализовать аргументы командной строки в Go?  
- Как Go обрабатывает зависимости?  
- В чем уникальное преимущество Go?  
- Что находится в каталоге src?  
- Назовите одну функцию Go, которая была бы полезна для DevOps.  
- Что заставляет Go быстро компилироваться?


---
### OZON

- шардирование по баккетам (применяется в ozon)
- консистентное хэширование, что это, как устроен
- как шардировать в продакшене для повышения отказоустойчивости
- шардирование и репликации
- паттерн консенсуса рафт
- вытеснение (алгоритмы)
- консистентность (минусы консистентности)
- worker pool/fanin
- что такое сага
- как гарантированно записать событие в базу данных и очередь (TransactionalOutbox)
- сервис 1 и сервис 2 общаются между собой через брокер сообщений - Kafka. Сервис 1 просит сервис 2 провести платёж (т.е. отправляет в Kafka сообщение с запросом на проведение платежа). Какие могут быть проблемы? 


---
- Go - Декларативный или Императивный яп?
- Обобщённые средства программирования в Golang?
- Slice vs array
- Map - рассказ про мапу (порядок получения по ключу случайный - рандом, сделанно намеренно из-за возможной "эвакуации")
- defer - что это, использование в Go. Вызывается до или после return? 
- Interfaces в Go. Примеры стандартных интерфейсов в Golang? (из коробки - error, read, write)
- Gorutine vs Thread - отличия. Какое максимальное кол-во горутин можно запустить? Кто ограничивает количество? Средства ограничения на создание горутин в языке (ограничивается оперативной памятью)?
- Каналы - что из себя представляет из себя канал внутри.
- Чтение из закрытого канала.
- Gracefull shutdown - рассказ (пименял ли и что это такое, как устроен)
- Какие библиотеки в Go применял для работы с базами двнных? (драйверы для бд типа - pgx)
- Что делает индексация баз данных. Виды индексов в postgresql. (b-tree, hash, gin-index, index-index)
- Как осуществлять анализ запросов в БД?
- gRPC + middleware приходилось ли осуществлять?
	- protoc  - делает и REST getaway и gRPC gateway
	- interceptors - перехватчики
- Grafana - как работал. (сам не разворачивал)
	- какие метрики собирал? (ram, cpu - Prometeus)
- Gitlab (playbook? ansible?)
- DDD? TDD? - приходилось ли использовать?

---
### Топ 3 частых вопросов

#### 3-тье место по частоте

1. Что такое транзакции и уровни изоляций транзакции (аномалии)
	книга - PostgreSQL 15 изнутри
2. Индексы, что это и зачем это надо? Вывод операции EXPLAIN
3. Kafka consumer, producer, topic, patition, consumer group, commit, ofset
4. что такое HTTP
5. что такое REST API
6. сравнить HTTP vs HTTPS, HTTP vs HTTP2, TCP vs UDP (гарантии доставки и последовательности), процесс установления TCP connection
----------- GO ------------
7. Что такое Rase Condition, DeadLock
8. Пакет sync (Mutex, RWMutex, SyncMap)
	Зачем нужна SyncMap исли обычную Map можно обернуть в mutex
9. Interfaces. 
	- Type assertion
	- Из чего состоит интерфейс (из двух частей)
	- Сравнение интерфейса с nil

#### 2-ое место

1. Gorutines, Channels
	- Что будет если написать в закрытый канал
	- Почему горутины легковестны
	- Сравнение горутин и системных тредов
	- config switch
	- Что такое stack (стэк). Где аллоцируется стэк?
	- Что такое heap (куча)
	- Как работает шедуллер (планировщик Go)
	- Про виды многозадачности и какая используется в го
	- Что такое вытесняющие и корпоративные задачи (многозадачность)
	
#### 1-ое место

1. Что такое Map
	- Сложность
	- Внутреннее устройство
	- Что может быть ключем Map
	- Почему не гарантирован порядок обхода
	- Эвакуация данных
	- Что такое бакет
	- Что такое экстра бакет
	- Что такое коллизии
	- Что происходит когда происходит много коллизий
2. Что такое Slice
	- как устроенны внутри
	- len, capacity. С каким коэфициентом увеличивается capacity
	- функция append, как работает
	- как slice передается в функцию 
	- функция copy, как работает, пожводные камни
3. Базовые вопросы по SQL (select, join, group by)
4. Внутреннее устройство сборщика мусора GC в Go
5. Контекст и graceful shutdown, расскажи про то и другое, как работают вместе

---
### Life Coding

1. Фибоначи. Дано число N - порядковый номер числа из последовательности фибоначи. Нужно вывести число под порядковым номером N.
	F(0)=0, F(1)=1, F(2)=1
```go
func fib(n int) int {
	if n = 0 {
		return 0
	}
	if n = 1 {
		return 1
	}

	a, b := 0, 1
	for i := 2; i <= n; i++ {
		c := a + b
		a = b
		b = c 
	}
	
	return b 
} 
```

2. Данн массив целых чисел. Вывести true - если есть повторяющиеся числа.
```go
func containsDuplicate(nums []int) bool {
	u := make(map[int]int)
	for _, n := range nums {
		u[n]++
	}
	
	for _, v := range u {
		if v > 1 {
			return true
		}
	}
	return false 
}
```

3. Нужно сгенерировать случайную матрицу размером NxM с неповторяющимися элементами.
```go
package main

import "fmt"
import "math/rand"

func generate(n, m int) [][]int {
	res := make([][]int, n)
	u := make(map[int]struct{})
	for i := 0; i < n; i++ {
		row := make([]int, m)
		res[i] = row
		for j := 0; j < m; j++ {
			for {
				v := rand.Int()
				_, ok := u[v]
				if !ok {
					row[j] = v
					u[v] = struct{}{}
					break
				}
			}
		}
	}
	return res
}

func main() {
	fmt.Println(generate(2,3))
}
```

4. Нужно смёржить несколько массивов в один массив.
```go
package main

import "fmt"

func merge(arrs... []int) []int {
	length := 0
	for _, arr := range arrs {
		length += len(arr)
	}

	res := make([]int, length)
	h := res[0:length]
	for _, arr := range arrs {
		copy(h, arr)
		h = h[len(arr):]
	}
	return res
}

func main() {
	fmt.Println(merge([]int{1,2}, []int{3,4}, []int{5,6})
}

```



---
### Тестовые задания

- Тестовое задание сокращатель ссылок [Google Drive](https://docs.google.com/document/d/1MyvuLON5YLCXjQXjy15BldndL0oaBs22RnjZ_ZMIRn0/edit?tab=t.0)
- Тестовое задание [Google Drive](https://docs.google.com/document/d/18HiyjzJH1A90tWDZpNTdv_GY_dT8Dmww0M4oD83eD1w/edit?pli=1&tab=t.0)
- Тестовое задание. Aviasales [Google Drive](https://docs.google.com/document/d/1gKlIZUeQRfTIX4GJv8bHjiEsXyosD2h8ztsUn9x0jpU/edit?tab=t.0)
- Нужно разработать микросервис для счетчиков статистики. Сервис должен уметь взаимодействовать с клиентом при помощи REST API или JSON RPC запросов. Также нужно реализовать валидацию входных данных. [Google Drive](https://docs.google.com/document/d/1IpToIj1VYEITPzrb-jjvGRn-XgwfYksiLRSOkX7p7jc/edit?tab=t.0)
- Задача - нужно написать сервис который по http будет работать как кеш. Ему можно закинуть пару ключ-значение и он его сохранит, также можно запросить значение по ключу - и если оно есть, должен отдать. Далее вопросы как его сделать многопоточным и быстрым.
- Нужно реализовать HTTP сервис для голосования. Например, для выбора самого популярного покемона.[Google Drive](https://docs.google.com/document/d/129xeMQxhGbK4YEQwzUJvjEuQnRTEdcVzCuxAvb9z3cU/edit?tab=t.0)

### Live Coding

- **Что выведется в результате исполнения кода? Как поправить?**

```go
package main

import (
 "fmt"
)

type myError struct {
 code int
}

func (e myError) Error() string {
 return fmt.Sprintf("code: %d", e.code)
}

func run() error {
 var e *myError
 if false {
  e = &myError{code: 123}
 }
 return e
}

func main() {
 err := run()
 if err != nil {
  fmt.Println("failed to run, error: ", err)
 } else {
  fmt.Println("success")
 }
}
```
Какие еще ошибки бывают в go?

- **Есть схема базы данных ниже. Нужно вывести все города и сколько людей в них живут одним запросом**

```sql
create table cities (
 id  serial primary key,
   name  text not null
);

insert into cities
 (name)
values
 ('Москва'),
 ('Санкт-Петербург'),
 ('Краснодар');

create table users (
 id  serial primary key,
   name  text not null,
  city_id int  not null references cities (id)
);

insert into users
 (name, city_id)
values
 ('Иван', 1),
 ('Анна', 1),
 ('Олег', 2);
```



-  **Как распараллелить запросы из следующего кода чтобы они шли одновременно.**

```go
package main

import (
 "fmt"
 "time"
)

const numRequests = 1000

var count int

func networkRequest() {
 time.Sleep(time.Millisecond)

 count++
}

func main() {
 defer timer()()

 for i := 0; i < numRequests; i++ {
  networkRequest()
 }
}

func timer() func() {
 start := time.Now()

 return func() {
  fmt.Printf("count %v took %v\n", count, time.Since(start))
 }
}
```


-  **Задача на логику - есть кольцевой поезд, в каждом вагоне горит или не горит лампочка и есть выключатель. Надо понять сколько вагонов в поезде.**
 
-  **Вопрос:**
	**1:** Хотим набрать строку s из строчных латинских букв в текстовом редакторе, который поддерживает два типа операций: дописать любую букву в конец; скопировать существующую подстроку и вставить ее в конец. Можно ли набрать s за число операций, меньшее длины строки? 
	"tbank" = false 
	"mama" = true
	"aaa" = false
	
	**2:** Есть массив из неотриц. чисел. Найти подмассив с макс суммой, в котором не более двух чисел. Вернуть сумму. 
	maxSum([1, 2]) = 3
	maxSum([10, 10, 3, 5, 5, 5]) == 23 (10, 10, 3)

- **Вопрос:**
	Нужно вывести 10 цифр в консоль. Потом нужно сделать это параллельно. Потом это нужно сделать одновременно, но сохраняя порядок.

```go
package main

import (
 "fmt"
 "time"
)

func main() {

}

func printNumber(n int) {
 time.Sleep(time.Second)
 fmt.Println(n)
}
```

- **Что выведется в результате выполнения кода?**

```go
func main() {
	runtime.GOMAXPROCS(1)
	var wg sync.WaitGroup
	wg.Add(1)
	go func() {
		time.Sleep(time.Second * 2)
		fmt.Println("1")
		wg.Done()
	}()
	go func() {
		fmt.Println("2")
	}()
	wg.Wait()
	fmt.Println("3")
}  
```

- **Вопрос:**
	У нас есть твиттер, пользователь заходит, видит твит что Месси забил сотый гол. Потом он обновляет ленту, а запись пропадает. Он сходил на другую реплику - там где этой записи еще нет. Как сделать чтобы пользователь всегда видел один фид?

- **Вопрос:**
	Что выведется в результате исполнения кода?
	
```go
func main() { 
	a := 0
	defer fmt.Println(a) 
	defer func() {
		a++  
		fmt.Println(a)
	}()
}
```

- **Вопрос:**
	Нужно задизайнить сервис, который ведет учет балансов и операций пополнения и снятия со счета. У него три grpc ручки - get balance, получить депозит и отправить деньги. Мы должны реализовать операции и записывать лог. Как бы с точки зрения схемы данных в базе данных приложение бы выглядело? Опиши логику приложения.

- **Почему по i, j ходить быстро а по j, i медленно?**

- **Напишите функцию для обращения односвязного списка.**
```go
package main

import "fmt"

type Node struct {
    Value int
    Next  *Node
}

func reverseList(head *Node) *Node {
    var prev *Node // предыдущий узел, изначально nil
    current := head // текущий узел
    for current != nil { // пока не конец списка
        next := current.Next // сохраняем следующий узел
        current.Next = prev  // меняем ссылку на предыдущий
        prev = current       // сдвигаем prev
        current = next       // сдвигаем current
    }
    return prev // новый head — последний узел
}

func main() {
    // Создаем список: 1 -> 2 -> 3
    list := &Node{1, &Node{2, &Node{3, nil}}}
    reversed := reverseList(list)
    for n := reversed; n != nil; n = n.Next {
        fmt.Print(n.Value, " ") // 3 2 1
    }
}
```

- **Что такое LRU cache. Реализуй LRU cache.**
```go
package main

import (
    "fmt"
    "sync"
)

// Node — узел двусвязного списка для хранения ключа и значения
type Node struct {
    Key   int   // ключ элемента
    Value int   // значение элемента
    Prev  *Node // указатель на предыдущий узел
    Next  *Node // указатель на следующий узел
}

// LRUCache — структура кэша
type LRUCache struct {
    capacity int            // максимальная емкость кэша
    cache    map[int]*Node // хэш-таблица для быстрого доступа по ключу
    head     *Node         // голова списка (самый недавно использованный)
    tail     *Node         // хвост списка (наименее недавно использованный)
    mu       sync.Mutex    // мьютекс для конкурентной безопасности
}

// NewLRUCache — конструктор для создания нового LRUCache
func NewLRUCache(capacity int) *LRUCache {
    return &LRUCache{
        capacity: capacity,
        cache:    make(map[int]*Node),
        head:     nil, // изначально список пуст
        tail:     nil,
    }
}

// Get — получение значения по ключу
func (c *LRUCache) Get(key int) int {
    c.mu.Lock()         // блокируем доступ для безопасности
    defer c.mu.Unlock() // разблокируем после завершения

    // ищем ключ в хэш-таблице
    if node, ok := c.cache[key]; ok {

        // если ключ найден, перемещаем элемент в голову (недавно использован)
        c.moveToHead(node)
        return node.Value // возвращаем значение
    }
    return -1 // если ключ не найден, возвращаем -1
}

// Put — добавление или обновление элемента в кэше
func (c *LRUCache) Put(key, value int) {
    c.mu.Lock()         // блокируем доступ
    defer c.mu.Unlock() // разблокируем после завершения

    // проверяем, существует ли ключ
    if node, ok := c.cache[key]; ok {
        // если ключ есть, обновляем значение и перемещаем в голову
        node.Value = value
        c.moveToHead(node)
        return
    }

    // создаем новый узел
    newNode := &Node{Key: key, Value: value}

    // добавляем в хэш-таблицу
    c.cache[key] = newNode

    // если список пуст, инициализируем head и tail
    if c.head == nil {
        c.head = newNode
        c.tail = newNode
    } else {
        // добавляем в начало списка
        newNode.Next = c.head
        c.head.Prev = newNode
        c.head = newNode
    }

    // проверяем емкость
    if len(c.cache) > c.capacity {
        // удаляем наименее используемый элемент (tail)
        c.removeTail()
    }
}

// moveToHead — перемещение узла в начало списка (недавно использованный)
func (c *LRUCache) moveToHead(node *Node) {
    // если узел уже в голове, ничего не делаем
    if node == c.head {
        return
    }

    // отсоединяем узел от текущей позиции
    if node.Prev != nil {
        node.Prev.Next = node.Next
    }
    if node.Next != nil {
        node.Next.Prev = node.Prev
    }

    // если узел был хвостом, обновляем tail
    if node == c.tail {
        c.tail = node.Prev
    }

    // перемещаем в голову
    node.Next = c.head
    node.Prev = nil
    c.head.Prev = node
    c.head = node
}

// removeTail — удаление наименее используемого элемента (хвоста)
func (c *LRUCache) removeTail() {
    if c.tail == nil {
        return
    }

    // удаляем ключ из хэш-таблицы
    delete(c.cache, c.tail.Key)

    // обновляем tail
    c.tail = c.tail.Prev
    if c.tail != nil {
        c.tail.Next = nil
    } else {
        // если список стал пустым, сбрасываем head
        c.head = nil
    }
}

func main() {
    // создаем LRU Cache с емкостью 2
    cache := NewLRUCache(2)

    // добавляем элементы
    cache.Put(1, 1) // [1]
    cache.Put(2, 2) // [2, 1]
    fmt.Println(cache.Get(1))       // 1, [1, 2]
    cache.Put(3, 3) // [3, 1], 2 удален
    fmt.Println(cache.Get(2))       // -1 (не найден)
    cache.Put(4, 4) // [4, 3], 1 удален
    fmt.Println(cache.Get(1))       // -1 (не найден)
    fmt.Println(cache.Get(3))       // 3, [3, 4]
    fmt.Println(cache.Get(4))       // 4, [4, 3]
}
```

- **Реализуйте конкурентный обход массива с суммированием элементов.**

```go
package main

import (
    "fmt"
    "sync"
)

func sumPart(arr []int, start, end int, result chan<- int, wg *sync.WaitGroup) {
    defer wg.Done()
    sum := 0
    for i := start; i < end; i++ { // суммируем часть массива
        sum += arr[i]
    }
    result <- sum // отправляем результат в канал
}

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    numWorkers := 4               // количество горутин
    result := make(chan int, numWorkers) // канал для результатов
    var wg sync.WaitGroup

    // делим массив на части
    chunkSize := (len(arr) + numWorkers - 1) / numWorkers
    for i := 0; i < numWorkers; i++ {
        start := i * chunkSize
        end := start + chunkSize
        if end > len(arr) {
            end = len(arr)
        }
        if start < len(arr) {
            wg.Add(1)
            go sumPart(arr, start, end, result, &wg)
        }
    }

    // закрываем канал после завершения горутин
    go func() {
        wg.Wait()
        close(result)
    }()

    // суммируем результаты
    total := 0
    for sum := range result {
        total += sum
    }
    fmt.Println("Общая сумма:", total) // Общая сумма: 55
}
```

**Как бы вы ограничили количество одновременно работающих горутин?**
- Задача: Реализовать обработку 100 задач с максимум 5 горутинами.
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, tasks <-chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for task := range tasks { // читаем задачи из канала
        fmt.Printf("Работник %d обрабатывает задачу %d\n", id, task)
        time.Sleep(100 * time.Millisecond) // имитация работы
    }
}

func main() {
    tasks := make(chan int, 10) // буферизированный канал для задач
    var wg sync.WaitGroup

    // запускаем 5 работников (горутин)
    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, tasks, &wg)
    }

    // отправляем 100 задач в канал
    for i := 1; i <= 100; i++ {
        tasks <- i
    }
    close(tasks) // закрываем канал после отправки всех задач

    wg.Wait() // ждем завершения всех работников
    fmt.Println("Все задачи обработаны")
}
```

- **Как бы вы реализовали конкурентный счетчик?**

```go
package main

import (
    "fmt"
    "sync"
)

type Counter struct {
    value int
    mu    sync.Mutex // мьютекс для защиты доступа к value
}

func (c *Counter) Increment() {
    c.mu.Lock()   // блокируем доступ к value
    c.value++     // увеличиваем счетчик
    c.mu.Unlock() // разблокируем
}

func (c *Counter) Get() int {
    c.mu.Lock()       // блокируем для безопасного чтения
    defer c.mu.Unlock() // гарантируем разблокировку после чтения
    return c.value    // возвращаем текущее значение
}

func main() {
    counter := Counter{}
    var wg sync.WaitGroup

    // запускаем 100 горутин для конкурентного увеличения
    for i := 0; i < 100; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            counter.Increment() // безопасно увеличиваем счетчик
        }()
    }

    wg.Wait() // ждем завершения всех горутин
    fmt.Println("Итог:", counter.Get()) // выводим результат: 100
}
```



-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
