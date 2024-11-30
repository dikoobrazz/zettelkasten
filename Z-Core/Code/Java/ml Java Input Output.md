2022120419:42
___
Date: 04-12-2022 | 19:42
Tags: #code #java #io 
mapofcontents: 
___
## Теория

Потоки ввода/вывода используются для передачи данных в файловые потоки, на консоль или на сетевые соединения. Потоки представляют собой объекты соответствующих классов. Пакеты 
ввода/вывода java.io, java.nio предоставляют пользователю большое число классов и методов и постоянно обновляются.

Все потоки ввода последовательности байтов являются подклассами абстрактного класса **InputStream**, потоки вывода — подклассами абстрактного класса **OutputStream**. При работе с файлами используются подклассы этих классов, соответственно, [[00 FileInputStream|FileInputStream]] и [[00 FileOutputStream|FileOutputStream]], конструкторы которых открывают поток и связывают его с соответствующим физическим файлом. Существуют классы-потоки для ввода массивов байтов, строк, объек-
тов, а также для выбора данных из файлов и сетевых соединений.
При взаимодействии с информационными потоками возможны различные исключительные ситуации, поэтому обработка исключений вида [[00 Try-Catch block|try-catch]] при использовании методов чтения и записи является ==обязательной==.


![[_resources/java-io-flow.png]]

### Java OutputStream class
Класс **OutputStream** является абстрактным классом. Это надкласс всех классов, представляющих выходной поток байтов. Выходной поток принимает выходные байты и отправляет их в некоторый приемник.

==Иерархия потока вывода==

![[_resources/java-outputstream.png|700]]

### Java InputStream class 
Класс **InputStream** является абстрактным классом. Это надкласс всех классов, представляющих входной поток байтов.

==Иерархия входного потока==

![[_resources/java-inputstream.png]]


Для обработки символьных потоков в формате Unicode применяется отдельная иерархия подклассов абстрактных классов [[00 Reader|Reader]] и [[00 Writer|Writer]], которые почти полностью повторяют функциональность байтовых потоков, но являются более актуальными при передаче текстовой информации.
Например, аналогом класса [[00 FileInputStream|FileInputStream]] является класс [[00 FileReader|FileReader]].

==Иерархия символьных потоков ввода==

![[_resources/Pasted image 20221211143431.png]]

==Иерархия символьных потоков вывода==

![[_resources/Pasted image 20221211143517.png]]


1. Шилдт - главы 13, 15, 20, 21, 22, 29
1. [Основные отличия Java IO и Java NIO](https://habr.com/ru/post/235585/)
2. [Файлы и работа с ними.](https://proselyte.net/tutorials/java-core/files-io/)
3. [Файлы и работа с ними. ByteArrayOutputStream.](https://proselyte.net/tutorials/java-core/files-io/byte-array-output-stream/)
4. [Файлы и работа с ними. ByteArrayInputStream.](https://proselyte.net/tutorials/java-core/files-io/byte-array-input-stream/)
5. [Файлы и работа с ними. DataInputStream](https://proselyte.net/tutorials/java-core/files-io/data-input-stream/)
6. [Сериализация](https://proselyte.net/tutorials/java-core/serialization/)
7. [Работа с AWS S3](https://proselyte.net/typical-tasks/aws/aws-s3/)

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 FileInputStream]]
- [[00 FileOutputStream]]
- [[00 BufferedInputStream]]
- [[00 BufferedOutputStream]]
- [[00 ByteArrayOutputStream]]
- [[00 ByteArrayInputStream]]
- [[00 DataOuputStream]]
- [[00 DataInputStream]]
- [[00 Reader]]
- [[00 Writer]]
- [[00 FileReader]]
- [[00 FileWriter]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-io)
- [Proselyte](https://proselyte.net/tutorials/java-core/files-io/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/package-summary.html)
