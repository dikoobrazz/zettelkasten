2022120712:06
___
Date: 07-12-2022 | 12:06
Tags: #code #java #collections 
mapofcontents:
___
## Java PriorityQueue Class
PriorityQueue также является классом, определенным в _collection framework_, который дает нам способ обработки объектов на основе приоритета. Уже было [[00 Queue|описано]], что вставка и удаление объектов осуществляется по шаблону _FIFO_. Однако иногда элементы очереди необходимо обрабатывать в соответствии с приоритетом, и здесь вступает в действие PriorityQueue.

PriorityQueue основан на куче приоритетов. Элементы приоритетной очереди упорядочиваются в соответствии с естественным порядком или компаратором, предоставленным во время построения очереди, в зависимости от того, какой конструктор используется.

### PriorityQueue Class Declaration
```java
public class PriorityQueue<E> extends AbstractQueue<E> implements Serializable
```
### Вот несколько важных моментов в Priority Queue:
- PriorityQueue не допускает null. 
- Мы не можем создать PriorityQueue из несопоставимых объектов. 
- PriorityQueue — несвязанные очереди. 
- _Head_ очереди является наименьшим элементом по отношению к указанному порядку. Если несколько элементов привязаны к наименьшему значению, голова является одним из этих элементов — связи разрываются произвольно. 
- Поскольку PriorityQueue не является потокобезопасным, java предоставляет класс [[00 PriorityBlockingQueue|PriorityBlockingQueue]], который реализует интерфейс [[00 BlockingQueue|BlockingQueue]] для использования в многопоточной среде java. 
- Операции извлечения из очереди опрашивают, удаляют, просматривают и получают доступ к элементу в начале очереди. 
- PriorityQueue обеспечивает время O(log(n)) для методов добавления и опроса. 
- PriorityQueue наследует методы класса AbstractQueue, AbstractCollection, [[00 Collection|Collection]] и Object.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Queue]]
- [[00 Deque]]
- [[00 BlockingQueue]]
- [[00 PriorityBlockingQueue]]
- [[00 Collection]]
- [[ml Java Collections API]]
------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-priorityqueue)
- [GeeksForGeeks](https://www.geeksforgeeks.org/priority-queue-class-in-java/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/PriorityQueue.html)
