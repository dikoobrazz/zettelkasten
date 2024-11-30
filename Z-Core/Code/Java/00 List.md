2022120515:02
___
Date: 05-12-2022 | 15:02
Tags: #code #java #collections 
mapofcontents:
___
## Java List Interface


![[_resources/List.png]]


Список (List) в Java предоставляет возможность поддерживать упорядоченную коллекцию. Он содержит основанные на индексах методы для вставки, обновления, удаления и поиска элементов. Он также может иметь повторяющиеся элементы. Мы также можем хранить нулевые элементы в списке.
Интерфейс List находится в пакете java.util и наследует интерфейс [[00 Collection|Collection]]. Это фабрика интерфейса ListIterator. С помощью ListIterator мы можем перебирать список в прямом и обратном направлениях. Классами реализации интерфейса List являются [[00 ArrayList|ArrayList]], [[00 LinkedList|LinkedList]], Stack и Vector. ArrayList и LinkedList широко используются в программировании на Java. Класс Vector устарел, начиная с Java 5.

### List Interface declaration
```java
public interface List<E> extends Collection<E>
```

### List< E > extends Collection< E >

-   `boolean addAll(int index, Collection<? extends E> c);`
-   `void sort(Comparator<? super E> c);`
-   `E get(int index);`
-   `E set(int index, E element);`
-   `void add(int index, E element);`
-   `E remove(int index); / boolean remove(Object o);`
-   `int indexOf(Object o);`
-   `int lastIndexOf(Object o);`
-   `ListIterator<E> listIterator();`
-   `ListIterator<E> listIteraor(int index);`
-   `List<E> subList(int fromIndex, int toIndex);`

### ListIterator< E > extends Iterator< E >  

-   `boolean hasPrevious();`
-   `E previous();`
-   `int nextIndex();`
-   `int previousIndex();`
-   `void set(E e);`
-   `void add(E e);`  

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 ArrayList]]
- [[00 LinkedList]]
- [[00 Array]]
- [[00 Collection]]
- [[ml Java Collections API]]
------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-list)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)
