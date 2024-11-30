2022120520:53
___
Date: 05-12-2022 | 20:53
Tags: #code #java #collections 
mapofcontents:
___
## Java LinkedHashSet Class
Класс Java LinkedHashSet представляет собой реализацию Hashtable и [[00 LinkedList|LinkedList]] интерфейса [[00 Set|Set]]. Он наследует класс [[00 HashSet|HashSet]] и реализует интерфейс Set.
Этот класс запоминает порядок добавления элементов.
Поэтому, когда мы захотим увидеть содержимое структуры, то элементы будут выведены в том порядке, в котором они были добавлены.


![[_resources/linkedhashset.png]]

> [!warning] Caution!
> Сохранение порядка вставки в LinkedHashset связано с некоторыми дополнительными затратами, как с точки зрения дополнительной памяти, так и с точки зрения дополнительных циклов ЦП. Поэтому, если не требуется поддерживать порядок вставки, вместо этого используйте более легкий [[00 HashMap|HashMap]] или [[00 HashSet|HashSet]].

### Важные моменты в классе Java LinkedHashSet: 
- Класс Java LinkedHashSet содержит уникальные элементы, подобные [[00 HashSet|HashSet]]. 
- Класс Java LinkedHashSet предоставляет все необязательные операции с множествами и допускает нулевые элементы. 
- Класс Java LinkedHashSet не синхронизирован. 
- Класс Java LinkedHashSet поддерживает порядок вставки.

### Иерархия класса LinkedHashSet 
Класс LinkedHashSet расширяет класс  [[00 HashSet|HashSet]], реализующий интерфейс [[00 Set|Set]]. 
Интерфейс Set наследует интерфейсы [[00 Collection|Collection]] и [[00 Iterable|Iterable]] в иерархическом порядке.

### LinkedHashSet Class Declaration
```java
public class LinkedHashSet<E> extends HashSet<E> 
implements Set<E>, Cloneable, Serializable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Set]]
- [[00 HashSet]]
- [[00 HashMap]]
- [[00 LinkedList]]
- [[00 Collection]]
- [[00 Iterable]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-linkedhashset)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashSet.html)
