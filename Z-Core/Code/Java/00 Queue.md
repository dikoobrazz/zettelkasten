2022120710:12
___
Date: 07-12-2022 | 10:12
Tags: #code #java #collections 
mapofcontents:
___
## Java Queue Interface
Интерфейс Queue доступен в пакете java.util и расширяет интерфейс [[00 Collection|Collection]]. Он используется для хранения элементов, которые обрабатываются в порядке поступления (FIFO). Это упорядоченный список объектов, где вставка элементов происходит в конец списка, а удаление элементов — из начала списка.

Будучи интерфейсом, Queue требует для объявления конкретный класс, и наиболее распространенными классами являются [[00 LinkedList|LinkedList]] и [[00 PriorityQueue|PriorityQueue]] в Java. Реализации, выполненные этими классами, не являются потокобезопасными. Если требуется реализация потокобезопасной реализации, [[00 PriorityBlockingQueue|PriorityBlockingQueue]] является доступным вариантом.

![[_resources/Queue-Deque.png|500]]

### Queue Interface Declaration
```java
public interface Queue<E> extends Collection<E>
```
### Особенности Queue
Ниже приведены некоторые важные особенности очереди.
- Концепция FIFO используется для вставки и удаления элементов из очереди. 
- Java Queue обеспечивает поддержку всех методов интерфейса [[00 Collection|Collection]], включая удаление, вставку и т. д. 
- [[00 PriorityQueue|PriorityQueue]], [[00 ArrayBlockingQueue|ArrayBlocingQueue]] и [[00 LinkedList|LinkedList]] — наиболее часто используемые реализации. 
- Исключение NullPointerException возникает, если в BlockingQueues выполняется какая-либо нулевая операция. 
- Oчереди, которые присутствуют в пакете _util_, известны как Unbounded Queues. 
- Очереди, присутствующие в пакете _util.concurrent_, называются Bounded Queues. 
- Все очереди, за исключением [[00 Deque|Deque]], облегчают удаление и вставку в начале и в конце очереди; соответственно. 

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова

- [[00 LinkedList]]
- [[00 PriorityQueue]]
- [[00 Deque]]
- [[00 ArrayDeque]]
- [[00 Stack]]
- [[00 BlockingQueue]]
- [[00 PriorityBlockingQueue]]
- [[00 ArrayBlockingQueue]]
- [[00 Collection]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javstpoint](https://www.javatpoint.com/java-priorityqueue)
- [Metanit](https://metanit.com/java/tutorial/5.7.php)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/Queue.html)
