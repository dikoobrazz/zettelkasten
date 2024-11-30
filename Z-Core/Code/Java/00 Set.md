2022120519:19
___
Date: 05-12-2022 | 19:19
Tags: #code #java #collections 
mapofcontents:
___
## Java Set Interface
Коллекция Set, не содержащая повторяющихся элементов. 
Более формально, наборы не содержат пары элементов `e1` и `e2`, таких что `e1.equals(e2)`, и не более одного нулевого элемента. 
Как следует из названия, этот интерфейс моделирует математическую абстракцию множеств.

![[_resources/hashset.png]]

Интерфейс Set расширяет интерфейс [[00 Collection|Collection]] и представляет набор уникальных элементов. 
Set не добавляет новых методов, только вносит изменения унаследованные. 
В частности, метод `add()` добавляет элемент в коллекцию и возвращает `true`, если в коллекции еще нет такого элемента.

![[_resources/SetDiagram.png]]

**SET UNION** ОБЪЕДИНЕНИЕ МНОЖЕСТВ
```java
Set<Integer> set1 = new HashSet<>(); // set1.add() -> 0,1,2,3,4,5
Set<Integer> set2 = new HashSet<>(); // set2.add() -> 2,4,5,6,7,8
set1.addAll(set2);                   // -> [0,1,2,3,4,5,6,7,8]
```

**SET INTERSECTION** ПЕРЕСЕЧЕНИЕ МНОЖЕСТВ
```java
Set<Integer> set1 = new HashSet<>(); // set1.add() -> 0,1,2,3,4,5
Set<Integer> set2 = new HashSet<>(); // set2.add() -> 2,4,5,6,7,8
set1.retainAll(set2);                // -> [2,3,4,5]
```

**SET SUBTRACT** РАЗНОСТЬ МНОЖЕСТВ
```java
Set<Integer> set1 = new HashSet<>(); // set1.add() -> 0,1,2,3,4,5
Set<Integer> set2 = new HashSet<>(); // set2.add() -> 2,4,5,6,7,8
set1.removeAll(set2);                // -> [0,1]
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 HashSet]]
- [[00 LinkedНashSet]]
- [[00 TreeSet]]
- [[00 Collection]]
- [[ml Java Collections API]]


------
**Links** (Внешние ссылки)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/Set.html)

