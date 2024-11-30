2022120521:49
___
Date: 05-12-2022 | 21:49
Tags: #code #java #collections 
mapofcontents:
___
## Java TreeSet class
Класс TreeSet является реализацией интерфейса [[00 Set|Set]] и реализует ==красно-чёрное дерево==.
Элементы в этой структуре данных хранятся в упорядоченном по возрастанию порядке.
Эта структура данных является крайне эффективной, когда нам необходимо получить доступ к элементу при большом количестве элементов.


![[_resources/treeset.png]]

### Важные моменты в классе Java TreeSet: 
- Класс Java TreeSet содержит уникальные элементы, как в [[00 HashSet|HashSet]]. 
- Доступ к классу Java TreeSet и время извлечения очень быстрое. 
- Класс Java TreeSet не допускает нулевой элемент. 
- Класс Java TreeSet не синхронизирован. 
- Класс Java TreeSet поддерживает возрастающий порядок. 
- Класс Java TreeSet может разрешать только те универсальные типы, которые сопоставимы. Например, интерфейс [[00 Comparable|Comparable]] реализуется классом StringBuffer.

### Внутренняя работа класса TreeSet 
> [!warning] Caution! 
> TreeSet реализуется с использованием бинарного дерева поиска, которое самобалансируется, как красно-черное дерево. Следовательно, такие операции, как поиск, удаление и добавление, занимают время O(log(N)). Причина этого кроется в самобалансирующемся дереве. Это делается для того, чтобы высота дерева никогда не превышала O(log(N)) для всех упомянутых операций. Следовательно, это одна из эффективных структур данных для хранения больших отсортированных данных, а также для выполнения операций над ними.

### Иерархия класса TreeSet 
Класс Java TreeSet реализует интерфейс NavigableSet. 
Интерфейс NavigableSet расширяет интерфейсы SortedSet, [[00 Set|Set]], [[00 Collection|Collection]] и [[00 Iterable|Iterable]] в иерархическом порядке.

### TreeSet Class Declaration
```java
public class TreeSet<E> extends AbstractSet<E> 
implements NavigableSet<E>, Cloneable, Serializable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Set]]
- [[00 HashSet]]
- [[00 Comparable]]
- [[00 Collection]]
- [[00 Iterable]]
- [[ml Java Collections API]]


------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-treeset)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/TreeSet.html)
