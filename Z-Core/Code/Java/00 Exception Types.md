2022120816:34
___
Date: 08-12-2022 | 16:34
Tags: #code #java #exception 
mapofcontents:
___
## Java Exception Types

![[_resources/exceptionSchema.png]]

В основном существует два типа исключений: **проверяемые** и **непроверяемые**. 
**Error** рассматривается как непроверяемое исключение. 
Согласно Oracle, существует три типа исключений, а именно: 
1. Проверяемое исключение 
2. Непроверяемое исключение 
3. Ошибка

### Проверяемое исключение (Checked Exception)
Это исключения, которые появляются во время компиляции программы.
Классы, которые напрямую наследуют класс **Throwable**, за исключением **RuntimeException** и **Error**, называются Проверяемыми исключениями. Например, IOException, SQLException и т.д.

### Непроверяемое исключение (Unchecked Exception)
Классы, наследующие **RuntimeException**, называются Непроверяемыми исключениями. 
Например, **ArithmeticException**, **NullPointerException**, **ArrayIndexOutOfBoundsException** и т.д. Непроверяемые исключения не проверяются во время компиляции, но проверяются во время выполнения.

### Ошибка (Error)
Ошибки, как правило, игнорируются компилятором, потому что с его точки зрения всё правильно. Другими словами, на момент компиляции всё должно работать исправно. Но во время выполнения программы что-то случается. Некоторыми примерами ошибок являются **OutOfMemoryError**, **VirtualMachineError**, **AssertionError** и т.д.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Exception Basics]]
- [[ml Java Exceptions]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/exception-handling-in-java)
- [Pproselyte](https://proselyte.net/tutorials/java-core/exceptions/)
