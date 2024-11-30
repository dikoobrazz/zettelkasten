2022120817:48
___
Date: 08-12-2022 | 17:48
Tags: #code #java #exception 
mapofcontents: 
___
## Java Multi-catch block
За блоком **try** может следовать один или несколько блоков **catch**. Каждый блок **catch** должен содержать отдельный обработчик исключений. Итак, если вам нужно выполнять разные задачи при возникновении разных исключений, используйте блок multi-catch.

Единовременно возникает только одно исключение и одновременно выполняется только один блок **catch**. 
Все блоки **catch** должны быть упорядочены от наиболее специфичных к наиболее общим, т.е. **catch** для **ArithmeticException** должен стоять перед **catch** **Exception**.

![[_resources/multiple-catch-block-in-java.png]]

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Try-Catch block]]
- [[00 Exception Basics]]
- [[ml Java Exceptions]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/multiple-catch-block-in-java)
