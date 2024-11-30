2022120510:31
___
Date: 05-12-2022 | 10:31
Tags: #code #java #collections 
mapofcontents:
___
## Связный список
![[_resources/linkedlist.png]]


> [!note] FAQ
> Класс LinkedList в Java использует **двусвязный список** для хранения элементов. 
> Он обеспечивает структуру данных связанного списка. 
> Он наследует класс AbstractList и реализует интерфейсы List и Deque.

### Важными моментами в Java LinkedList являются: 
- Класс Java LinkedList может содержать повторяющиеся элементы. 
- Класс Java LinkedList поддерживает порядок вставки. 
- Класс Java LinkedList не синхронизирован. 
- В классе Java LinkedList манипулирование выполняется быстро, поскольку не требуется никакого смещения. 
- Класс Java LinkedList можно использовать как список, стек или очередь.

![[_resources/linkedlist.webp]]

### LinkedList class declaration
```java
public class LinkedList<E> extends AbstractSequentialList<E> 
implements List<E>, Deque<E>, Cloneable, Serializable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Collections API]]
- [[00 Collection]]
- [[00 List]]

------
**Links** (Внешние ссылки)
- [JavaPoint](https://www.javatpoint.com/java-linkedlist)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html)
