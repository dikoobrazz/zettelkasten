2022120818:24
___
Date: 08-12-2022 | 18:24
Tags: #code #java #exception 
mapofcontents:
___
## Java throw Exception
В Java исключения позволяют нам писать код хорошего качества, в котором ошибки проверяются во время компиляции, а не во время выполнения, и мы можем создавать собственные исключения, упрощающие восстановление и отладку кода.

### Ключевое слово throw
Ключевое слово **throw** используется для явного генерирования исключения. Мы указываем объект исключения, который должен быть выброшен. Исключение имеет некоторое сообщение с описанием ошибки. Эти исключения могут быть связаны с вводом данных пользователем, сервером и т.д. Мы можем генерировать как "проверяемые", так и "непроверяемые" [[00 Exception Types|исключения]] в Java с помощью ключевого слова **throw**. Он в основном используется для создания пользовательского исключения. Мы также можем определить наш собственный набор условий и явно генерировать исключение, используя ключевое слово **throw**. Например, мы можем сгенерировать исключение **ArithmeticException**, если мы разделим одно число на другое число. Здесь нам просто нужно установить условие и сгенерировать исключение, используя ключевое слово **throw**.

**EXAMPLE**
```java
throw new exception_class("error message");
throw new IOException("sorry device error");
```

> [!note] NOTE
> Если мы выбрасываем "непроверяемое" исключение из метода, необходимо обработать исключение или добавить throws в объявлении метода.
> Если мы выбрасываем "проверяемое" исключение с помощью ключевого слова throw, необходимо обработать исключение с помощью блока catch или метод должен быть объявлен с ключевым словом throws.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Exception Types]]
- [[00 Throws keyword]]
- [[00 Exception Basics]]
- [[ml Java Exceptions]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/throw-keyword)
- [Proselyte](https://proselyte.net/tutorials/java-core/exceptions/)