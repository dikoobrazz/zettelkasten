2022120519:42
___
Date: 05-12-2022 | 19:42
Tags: #code #java #collections 
mapofcontents:
___
## Java Class HashSet
Класс Java HashSet используется для создания коллекции, которая использует хеш-таблицу для хранения(фактически экземпляром [[00 HashMap|HashMap]]). Он наследует класс AbstractSet и реализует интерфейс Set.
Распределение и хранение данных выполняется с использованием хеширования (создания уникального идентификатора – ключа, на основе значения элемента). 
Результатом хеширования является значение, которое называется хэш-код.

![[_resources/hashset.png]]

### Важные моменты в классе Java HashSet: 
- HashSet хранит элементы с помощью механизма, называемого хэшированием. 
- HashSet содержит только уникальные элементы. 
- HashSet допускает нулевое значение. 
- Класс HashSet не синхронизирован. 
- HashSet не поддерживает порядок вставки. Здесь элементы вставляются на основе их хэш-кода.
- HashSet — лучший подход для операций поиска. 
- Начальная емкость HashSet по умолчанию — 16, а коэффициент загрузки — 0,75.

### Разница между List и Set
[[00 List|List]] может содержать повторяющиеся элементы, тогда как Set содержит только уникальные элементы.

### Иерархия класса HashSet 
Класс HashSet расширяет класс AbstractSet, реализующий интерфейс Set. 
Интерфейс Set наследует интерфейсы [[00 Collection|Collection]] и [[00 Iterable|Iterable]] в иерархическом порядке.

### HashSet class declaration
```java
public class HashSet<E> extends AbstractSet<E> 
implements Set<E>, Cloneable, Serializable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Collections API]]
- [[00 Set]]
- [[00 List]]
- [[00 HashMap]]
- [[00 Collection]]
- [[00 ContractHashEquals]]
- [[00 Iterable]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-hashset)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/HashSet.html)
