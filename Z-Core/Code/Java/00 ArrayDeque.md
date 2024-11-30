2022120713:21
___
Date: 07-12-2022 | 13:21
Tags: #code #java #collections 
mapofcontents: 
___
## Java ArrayDeque class
Реализация интерфейса Deque с изменяемым размером массива. Array Deque не имеет ограничений по емкости. Он растёт по мере необходимости для поддержки использования. Он не потокобезопасен; при отсутствии внешней синхронизации он не поддерживает одновременный доступ нескольких потоков. Нулевые элементы запрещены. Этот класс, вероятно, будет быстрее, чем -[[00 Stack|Stack]], когда используется в качестве стека, и быстрее, чем [[00 LinkedList|LinkedList]], когда используется в качестве очереди.

### Важными моментами в классе ArrayDeque являются: 
- В отличие от [[00 Queue|Queue]], мы можем добавлять или удалять элементы с обеих сторон. 
- Нулевые элементы не допускаются в ArrayDeque. 
- ArrayDeque не является потокобезопасным из-за отсутствия внешней синхронизации. 
- ArrayDeque не имеет ограничений по емкости. 
- ArrayDeque быстрее, чем [[00 LinkedList|LinkedList]] и [[00 Stack|Stack]].

### ArrayDeque class declaration
```java
public class ArrayDeque<E> extends AbstractCollection<E> 
implements Deque<E>, Cloneable, Serializable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Queue]]
- [[00 Deque]]
- [[00 Stack]]
- [[00 LinkedList]]
- [[00 Collection]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-deque-arraydeque)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/ArrayDeque.html)
