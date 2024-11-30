2022120709:00
___
Date: 07-12-2022 | 09:00
Tags: #code #java #collections 
mapofcontents:
___
## Java Comparator interface 
Интерфейс Java Comparator используется для упорядочения объектов пользовательского класса. Этот интерфейс находится в пакете java.util и содержит 2 метода сравнения 
`(Object o1, Object o2)`. Он обеспечивает несколько последовательностей сортировки, т. е. можно сортировать элементы на основе любого члена данных. Например, `id`, `name`, `age` или что-либо еще.

### Collections Class
Класс Collections предоставляет статические методы для сортировки элементов коллекции. Если элементы коллекции относятся к [[00 Set|Set]] или [[00 Map|Map]], мы можем использовать [[00 TreeSet|TreeSet]] или [[00 TreeMap|TreeMap]]. Однако мы не можем сортировать элементы списка. Класс Collections также предоставляет методы для сортировки элементов элементов типа [[00 List|List]].

> [!note] FAQ!
> Класс Collections — это один из служебных классов в Java Collections Framework. Пакет java.util содержит класс Collections. Класс Collections в основном используется со статическими методами, которые работают с коллекциями или возвращают коллекцию

### Метод класса Collections для сортировки элементов списка
`public void sort(List list, Comparator c)` : используется для сортировки элементов списка по данному Comparator

**CODE EXAMPLE**
```java
public class Main {
	public static void main(String[] args) {
		List<Integer> numbers = new ArrayList<>();
		numbers.add(5); 
		numbers.add(0); 
		numbers.add(500);
		numbers.add(100);
		System.out.println(numbers);    // [5, 0, 500, 100]

		Collection.sort(numbers, new Comparator<Integer>(){
			@Override
			public int compare(Integer o1, Integer o2) {
				if(o1 > o2) return -1;
				else if (o1 < o2) return 1;
				else return 0;
			} 
		});

		System.out.println(numbers);    // [500, 100, 5, 0]
	}
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 List]]
- [[00 Set]]
- [[00 Map]]
- [[00 TreeSet]]
- [[00 TreeMap]]
- [[00 Comparable]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/Comparator-interface-in-collection-framework)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)
