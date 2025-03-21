2022120713:05
___
Date: 07-12-2022 | 13:05
Tags: #code #java #collections 
mapofcontents: 
___
## Java Deque Interface 
Интерфейс под названием Deque присутствует в пакете java.util. Это подтип очереди интерфейса. Deque поддерживает как просмотр,  добавление и удаление элементов с обоих концов структуры данных.
Каждый из этих методов существует в двух формах: один выдает исключение в случае сбоя операции, другой возвращает специальное значение (_null_ или _false_, в зависимости от операции). Последняя форма операции вставки разработана специально для использования с реализациями Deque с ограниченной емкостью. В большинстве реализаций операции вставки не могут завершиться ошибкой.
Deque можно использоваться как стек или очередь. Мы знаем, что _стек_ поддерживает операцию _"последний пришел — первый вышел"_ (LIFO), а операция _"первый пришел — первый вышел"_ поддерживается очередью. Поскольку _deque_ поддерживает и то, и другое, с ним можно выполнить любую из упомянутых операций. Deque — это аббревиатура от **"double ended queue"** (двухсторонняя очередь).

### Deque Interface declaration
```java
public interface Deque<E> extends Queue<E>
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Queue]]
- [[00 ArrayDeque]]
- [[00 Stack]]
- 

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-deque-arraydeque)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/Deque.html)
