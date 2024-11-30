2022120508:34
___
Date: 05-12-2022 | 08:34
Tags: #code #java #collections 
mapofcontents:
___
## Динамический массив
![[_resources/arraylist.png]]

**ArrayList** – это класс, который является реализацией динамического массива, т.е. массива, который при необходимости увеличивает свой размер.
Обычный [[00 Array|массив]] имеет фиксированный размер и после его создание количество элементов в нём не может быть увеличено.

> [!notes] FAQ
> ArrayList основан на Array (простом массиве) `Object[] elementData;` 
> При инициализации ArrayList, внутренний массив Array имеет размер - 10 
> `Object[] elementData = new Object[10]`

ArrayList в Java также может иметь повторяющиеся элементы. 
Он реализует интерфейс [[00 List|List]], поэтому мы можем использовать здесь все методы интерфейса List. ArrayList внутренне поддерживает порядок вставки. 
Он наследует класс AbstractList и реализует интерфейс List.

### Важные моменты в классе Java ArrayList: 
- Класс Java ArrayList может содержать повторяющиеся элементы. 
- Класс Java ArrayList поддерживает порядок вставки. 
- Класс Java ArrayList не синхронизирован. 
- Java ArrayList допускает произвольный доступ, поскольку массив работает на основе индекса.
- В ArrayList манипулирование происходит немного медленнее, чем в [[00 LinkedList|LinkedList]] в Java, потому что при удалении какого-либо элемента из списка массивов должно происходить много смещений. 
- Мы не можем создать список массивов примитивных типов, таких как int, float, char и т. д. В таких случаях необходимо использовать требуемый класс-оболочку.
- Java ArrayList инициализируется размером. Размер в списке массивов является динамическим и зависит от того, какие элементы добавляются или удаляются из списка.

### Иерархия класса ArrayList 
Как показано на диаграмме выше, класс Java ArrayList расширяет класс AbstractList, реализующий интерфейс [[00 List|List]]. Интерфейс List расширяет интерфейсы [[ml Java Collections API|Collection]] и Iterable в иерархическом порядке.

### ArrayList class declaration
```java
public class ArrayList<E> extends AbstractList<E> 
implements List<E>, RandomAccess, Cloneable, Serializable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Collections API]]
- [[00 Collection]]
- [[00 LinkedList]]
- [[00 List]]

------
**Links** (Внешние ссылки)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/ArrayList.html)
- [JvaPoint](https://www.javatpoint.com/java-arraylist)
