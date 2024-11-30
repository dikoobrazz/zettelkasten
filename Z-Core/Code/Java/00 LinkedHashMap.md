2022120518:22
___
Date: 05-12-2022 | 18:22
Tags: #code #java #collections 
mapofcontents:
___
## Java LinkedHashMap class
Класс LinkedHashMap хранит элементы типа "ключ – значение" в том порядке, в котором они были добавлены. Это означает, что когда мы захотим вывести содержимое структуры данных мы получим их не упорядоченными по возрастанию, а в порядке добавления.

![[_resources/linkedhashmap.png]]

### Важные моменты в классе LinkedHashMap
- Java LinkedHashMap содержит значения на основе ключа. 
- Java LinkedHashMap содержит уникальные элементы. 
- Java LinkedHashMap может иметь один нулевой ключ и несколько нулевых значений. 
- Java LinkedHashMap не синхронизирован. 
- Java LinkedHashMap поддерживает порядок вставки. 
- Начальная емкость класса Java HashMap по умолчанию составляет 16 с коэффициентом загрузки 0,75.

### LinkedHashMap class declaration
```java
public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V>
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Map]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-linkedhashmap)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html)
