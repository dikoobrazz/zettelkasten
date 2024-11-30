2022120713:36
___
Date: 07-12-2022 | 13:36
Tags: #code #java #collections 
mapofcontents:
___
## Java ArrayBlockingQueue Class
Класс ArrayBlockingQueue представляет собой **bounded blocking queue**, поддерживаемую [[00 Array|Array]]. Под ограниченным подразумевается фиксированный размер очереди. После создания емкость не может быть изменена. Попытки поместить элемент в полную очередь приведут к блокировке операции. Точно так же будут блокироваться попытки взять элемент из пустой очереди. Привязанность ArrayBlockingQueue может быть изначально достигнута путем обхода емкости в качестве параметра в конструкторе ArrayBlockingQueue. Эта очередь упорядочивает элементы **FIFO (first-in-first-out)**. Это означает, что **head** этой очереди является самым старым элементом из элементов, присутствующих в этой очереди.

Хвост этой очереди является самым новым элементом элементов этой очереди. Вновь вставленные элементы всегда вставляются в хвост очереди, а операции извлечения из очереди получают элементы в начале очереди. Этот класс и его итератор реализуют все необязательные методы интерфейсов [[00 Collection|Collection]] и [[00 Iterator|Iterator]]. Этот класс является членом Java Collections Framework.

![[_resources/ArrayBlockingQueue.png|500]]

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Array]]
- [[00 Queue]]
- [[00 Collection]]
- [[00 Iterator]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [GeeksForGeeks](https://www.geeksforgeeks.org/arrayblockingqueue-class-in-java/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ArrayBlockingQueue.html)
