2022120522:08
___
Date: 05-12-2022 | 22:08
Tags: #code #java #collections 
mapofcontents:
___
## Java EnumMap class
Класс Java EnumMap - cпециализированная реализация [[00 Map|Map]] для использования с ключами типа [[00 Enum|Enum]].
Он наследует классы Enum и AbstractMap.
Все ключи в EnumMap должны исходить из одного типа Enum, который указывается явно или неявно при создании Map. 
EnumMap внутренне представлены в виде [[00 Array|Array]]. Это представление чрезвычайно компактно и эффективно.

### EnumMap class declaration
```java
public class EnumMap<K extends Enum<K>,V> extends AbstractMap<K,V> 
implements Serializable, Cloneable
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Map]]
- [[00 Enum]]
- [[00 Array]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-enummap)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/EnumMap.html)
