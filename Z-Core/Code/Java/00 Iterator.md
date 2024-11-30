2022120717:49
___
Date: 07-12-2022 | 17:49
Tags: #code #java #collections 
mapofcontents: 
___
## Java Iterator Interface
Java Iterator — это интерфейс, для последовательного перебора набора компонентов объекта Java. Его можно использовать в языке программирования Java, начиная с платформы Java 1.2 Collection Framework. Он принадлежит пакету java.util.
Хотя Java Iterator был представлен в Java 1.2, он по-прежнему не является самым старым инструментом для обхода элементов объекта [[00 Collection|Collection]]. Самый старый Iterator в языке программирования Java — это Enumerator, предшествовавший Iterator.
Итератор Java также известен как универсальный курсор Java, поскольку он подходит для всех классов платформы [[00 Collection|Collection]]. Итератор Java также помогает в таких операциях, как READ и REMOVE.

### Преимущества итератора Java
Итераторы в Java получили широкое распространение благодаря своим многочисленным преимуществам. Преимущества Java Iterator приведены ниже: 
- Пользователь может применить эти итераторы к любому из классов платформы [[00 Collection|Collection]]. 
- В Java Iterator мы можем использовать как операции чтения, так и операции удаления. 
- Если пользователь работает с циклом for, он не может модернизировать (добавить/удалить) коллекцию, тогда как, если он использует итератор Java, он может просто обновить коллекцию. 
- Итератор Java считается универсальным курсором для API коллекции. 
- Имена методов в итераторе Java очень просты и очень просты в использовании.

### Недостатки итератора Java 
Несмотря на многочисленные преимущества, у Java Iterator есть и ряд недостатков. Недостатки Java Iterator приведены ниже:
- Итератор Java сохраняет итерацию только в прямом направлении. Проще говоря, итератор Java — это однонаправленный итератор. 
- Замена и расширение нового компонента не одобрены Java Iterator. 
- В операциях CRUD итератор Java не поддерживает различные операции, такие как CREATE и UPDATE. 
- По сравнению с Spliterator, Java Iterator не поддерживает обход элементов в параллельном шаблоне, что означает, что Java Iterator поддерживает только последовательную итерацию. 
- По сравнению с Spliterator, Java Iterator не поддерживает более надежное выполнение для прохождения большого объема данных.

**CODE EXAMPLE**
```java
public class Main {
	public static void main(String[] args) {
		List<Integer> list = new ArrayList<>();
		list.add(1);
		list.add(2);
		list.add(3);

		// Before Java 5
		Iterator<Integer> iterator = list.iterator();
		while(iterator.hasNext())
			Sustem.out.println(iterator.next());

		// Java 5
		for(int x: list)
			System.out.println(x);
	}
}
```
-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Iterable]]
- [[00 Collection]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-iterator)
- [Baeldung](https://www.baeldung.com/java-iterator-vs-iterable#3)
- [Proselyte](https://proselyte.net/tutorials/java-core/collections-framework/iterator/)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)
