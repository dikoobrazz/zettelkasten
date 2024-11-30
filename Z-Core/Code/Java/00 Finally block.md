2022120818:04
___
Date: 08-12-2022 | 18:04
Tags: #code #java #exception 
mapofcontents:
___
## Java finally block
Блок **finally** — это блок, используемый для выполнения важного кода, такого как закрытие соединения и т.д. Блок **finally** в Java всегда выполняется независимо от того, обрабатывается исключение или нет. Следовательно, он содержит все необходимые операторы, которые необходимо вывести независимо от того, произошло исключение или нет. 
Блок **finally** следует за блоком [[00 Try-Catch block|try-catch]].

==Блок-схема блока finally==

![[_resources/java-finally-block.png]]
> [!note] NOTE
> Если необрабатывать исключение, перед завершением программы JVM выполняет блок finally (если есть).

### Зачем использовать блок finally? 
- Блок **finally** в Java можно использовать для размещения кода "**cleanup**", такого как закрытие файла, закрытие соединения и т.д. 
- Важные утверждения для печати можно поместить в блок **finally**.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Try-Catch block]]
- [[00 Exception Basics]]
- [[ml Java Exceptions]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/finally-block-in-exception-handling)
- [Proselyte](https://proselyte.net/tutorials/java-core/exceptions/)
