2022120517:46
___
Date: 05-12-2022 | 17:46
Tags: #code #java #collections 
mapofcontents:
___
## Java HashMap
Класс Java HashMap реализует интерфейс [[00 Map|Map]], который позволяет хранить пары ключ-значение, где ключи должны быть уникальными. Если попытаться вставить дубликат ключа, он заменит элемент соответствующего ключа. С помощью индекса ключа легко выполнять операции, такие как обновление, удаление и т. д.


![[_resources/hashmap.png]]

HashMap в Java похож на устаревший класс Hashtable, но он не синхронизирован. 
Это также позволяет хранить нулевые элементы, но должен быть только один нулевой ключ. Начиная с Java 5, обозначается как HashMap<K,V>, где K обозначает ключ, а V — значение. HashMap наследует класс AbstractMap и реализует интерфейс Map.

### Важные моменты в классе Java HashMap
- Java HashMap содержит значения на основе ключа. 
- Java HashMap содержит только уникальные ключи. 
- Java HashMap может иметь один нулевой ключ и несколько нулевых значений. 
- Java HashMap не синхронизирован. 
- Java HashMap не поддерживает порядок. 
- Начальная емкость класса Java HashMap по умолчанию составляет 16 с коэффициентом загрузки 0,75.

### HashMap class declaration
```java
public class HashMap<K,V> extends AbstractMap<K,V> 
implements Map<K,V>, Cloneable, Serializable
```

`HashMap<K, V> -> <K, V> - Map.Entry<K, V>`

```java
public class Main {
	public static void main(String[] args) {
		Map<Integer, String> map = new HashMap<>();
		map.put(1, "One");
		map.put(2, "Two");
		map.put(3, "Three");

		for(Map.Entry<Integer, String) entry: map.entrySet()) {
			System.out.println(entry.getKey() + ": " + entry.gteValue());
		}
	}
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Map]]
- [[00 ContractHashEquals]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-hashmap)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
