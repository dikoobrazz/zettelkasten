2022120819:37
___
Date: 08-12-2022 | 19:37
Tags: #
mapofcontents:
___
## Java throws keyword
Ключевое слово Java **throws** используется для объявления исключения. Он сообщает программисту, что может возникнуть исключение. Таким образом, программисту лучше предоставить код обработки исключений, чтобы можно было поддерживать нормальный ход программы.

### Syntax of Java throws
```java
return_type method_name() throws exception_class_name{  
	//method code  
}
```

### Какое [[00 Exception Types|исключение]] следует объявить?
Только **"Проверяемое"** (Checked Exception) исключение, потому что: 
- **"Непроверяемое"** (Unchecked Exception) исключение под нашим контролем, поэтому мы можем исправить наш код.
- **Error** - вне нашего контроля. Например, мы ничего не сможем сделать, если произойдет **VirtualMachineError** или **StackOverflowError**.

### Преимущество ключевого слова Java throws
Теперь [[00 Exception Types|Checked Exception]] можно распространять (пересылать в [[00 Stack|стек]] вызовов). 
Оно предоставляет информацию вызывающему методу об исключении.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Exception Types]]
- [[00 Throw exception]]
- [[00 Exception Basics]]
- [[ml Java Exceptions]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/throws-keyword-and-difference-between-throw-and-throws)
- [Proselyte](https://proselyte.net/tutorials/java-core/exceptions/)
