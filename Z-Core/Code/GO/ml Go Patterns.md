2025020402:55
___
Date: 04-02-2025 | 02:55
Tags: #go #pattern
mapofcontents:
___
## Patterns in Go

| Паттерн                                       | Когда использовать?                      | Реализация в Go                       |
| --------------------------------------------- | ---------------------------------------- | ------------------------------------- |
| **[[00 Go - Pattern Singletone\|Singleton]]** | Когда нужен единственный экземпляр       | `sync.Once` + глобальная переменная   |
| **[[00 Go - Pattern Factory\|Factory]]**      | Когда нужно скрыть создание объектов     | Функция-фабрика                       |
| **[[00 Go - Pattern Builder\|Builder]]**      | Для создания сложных объектов            | Методы с цепочкой вызовов             |
| **[[00 Go - Pattern Strategy\|Strategy]]**    | Для смены алгоритма во время выполнения  | Интерфейсы и структуры                |
| **[[00 Go - Pattern Observer\|Observer]]**    | Когда нужно уведомлять подписчиков       | Слайс observers                       |
| **[[00 Go - Pattern DI\|DI]]**                | Для гибкости и тестируемости кода        | Внедрение через конструктор структуры |
| **[[00 Go - Pattern Decorator\|Decorator]]**  | Для динамического добавления функционала | Обертки вокруг структур (wrapped)     |
| **[[00 Go - Pattern Pipeline\|Pipeline]]**    | Когда нужна обработка данных в потоках   | Каналы (channels)                     |


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Go - Pattern Singletone]]
- [[00 Go - Pattern Factory]]
- [[00 Go - Pattern Builder]]
- [[00 Go - Pattern Strategy]]
- [[00 Go - Pattern Observer]]
- [[00 Go - Pattern DI]]
- [[00 Go - Pattern Decorator]]
- [[00 Go - Pattern Pipeline]]

------
**Links** (Внешние ссылки)
- [Refactoring GURU](https://refactoring.guru/ru/design-patterns/catalog)
