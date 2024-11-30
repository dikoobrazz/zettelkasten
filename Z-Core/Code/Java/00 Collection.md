2022120522:24
___
Date: 05-12-2022 | 22:24
Tags: #code #java #collections 
mapofcontents:
___
## Java Collection Interface 
Этот интерфейс позволяет нам работать с группами объектов.
==Collection== — это группа объектов, которые известны как элементы. 
Корневой интерфейс в иерархии коллекций. Коллекция представляет собой группу объектов, называемых ее элементами. Некоторые коллекции позволяют дублировать элементы, а другие нет. Некоторые упорядочены, а другие неупорядочены. JDK не предоставляет каких-либо прямых реализаций этого интерфейса: он предоставляет реализации более конкретных подинтерфейсов, таких как [[00 Set|Set]] и [[00 List|List]]. Этот интерфейс обычно используется для передачи коллекций и управления ими там, где требуется максимальная универсальность.


![[_resources/java-collection-hierarchy 1.png]]


### Collection< E > extends Iterable< E >

-   `int size();`
-   `boolean isEmpty();`
-   `boolean contains(Object o);`
-   `boolean containsAll(Collection<?> c);`
-   `Object[] toArray();`
-   `<T> T[] toArray(T[] a);`
-   `boolean add(E e);`
-   `boolean remove(Object o);`
-   `boolean addAll(Collection<? extends E> c);`
-   `boolean removeAll(Collection<?> c);`
-   `boolean retainAll(Collection<?> c);`
-   `void clear();`

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Set]]
- [[00 List]]
- [[00 Queue]]
- [[00 Deque]]
- [[00 Comparable]]
- [[00 Comaprator]]
- [[00 Iterable]]
- [[00 Iterator]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-collection)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)
