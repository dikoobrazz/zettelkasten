2022120518:41
___
Date: 05-12-2022 | 18:41
Tags: #code #java #collections 
mapofcontents:
___
## Java TreeMap class
Класс Java TreeMap представляет собой реализацию на основе красно-черного дерева. 
Он обеспечивает эффективное средство хранения пар ключ-значение в ==отсортированном== порядке по ключу в естественном порядке.

![[_resources/treemap.png]]

### Важные моменты в классе Java TreeMap: 
- Java TreeMap содержит значения на основе ключа. Он реализует интерфейс NavigableMap и расширяет класс AbstractMap. 
- Java TreeMap содержит только уникальные элементы. 
- Java TreeMap не может иметь нулевой ключ, но может иметь несколько нулевых значений. 
- Java TreeMap не синхронизирован. 
- Java TreeMap поддерживает восходящий порядок.

### TreeMap class declaration
```java
public class TreeMap<K,V> extends AbstractMap<K,V> 
implements NavigableMap<K,V>, Cloneable, Serializable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Map]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-treemap)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/TreeMap.html)
