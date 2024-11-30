2022120709:32
___
Date: 07-12-2022 | 09:32
Tags: #code #java #collections 
mapofcontents:
___
## Java Comparable interface
Интерфейс Java Comparable используется для упорядочения объектов пользовательского класса. Этот интерфейс находится в пакете java.lang и содержит только один метод с именем `compareTo(Object)`. Он обеспечивает только одну последовательность сортировки, т. е. можно сортировать элементы только на основе одного члена данных. Например, это может быть `id`, 
`name`, `age` или что-то еще.

[[00 List|Списки]] (и [[00 ArrayList|массивы]]) объектов, реализующих этот интерфейс, могут автоматически сортироваться с помощью `Collections.sort` (и `Arrays.sort`). Объекты, реализующие этот интерфейс, могут использоваться как ключи в отсортированной [[00 Map|Map]] или как элементы в отсортированном [[00 Set|Set]] без необходимости указывать [[00 Comaprator|Comparator]].

### compareTo(Object obj) method
`public int compareTo(Object obj)` : используется для сравнения текущего объекта с указанным объектом. Он возвращает:
- положительное целое число, если текущий объект больше указанного объекта.
- отрицательное целое число, если текущий объект меньше указанного объекта.
- ноль, если текущий объект равен указанному объекту.

Мы можем отсортировать элементы: 
- Строковые объекты 
- Объекты класса-обёртки 
- Пользовательские объекты класса

**CODE EXAMPLE**
```java
public class Main {
	public static void main(String[] args) {	
		List<Person> peopleList = new ArrayList<>();
		Set<Person> peopleSet = new TreeSet<>();

		addElements(peopleList);// [Person{id=2, name='To'}, Person{id=1, name='Bob'} ...]
		addElements(peopleSet); // [Person{id=2, name='To'}, Person{id=1, name='Bob'} ...]
	}
	private static void addElement(Collection collection) {
		collection.add(new Person(1, "Bob"));
		collection.add(new Person(2, "To"));
		collection.add(new Person(3, "Katy"));
		collection.add(new Person(4, "George"));
	}
}

class Person implements Comparable<Person>{
	private int id;
	private String name;
	
	private Person(int id, String name) {...}

	public Getter(), Setter() {...}

	@Override
	public boolean equals(Object o) {...}

	@Override
	public int hashCode() {...}

	@Override
	public String toString() {...}

	@Override
	public int compareTo(Person o) {
		if (this.name.length() > o.getName().length()) return 1;
		else if (this.name.length() < o.getName().length()) return -1;
		else return 0;
	}
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 List]]
- [[00 ArrayList]]
- [[00 Comaprator]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/Comparable-interface-in-collection-framework)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)
