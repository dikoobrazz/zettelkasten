2022121116:41
___
Date: 11-12-2022 | 16:41
Tags: #code #java #io 
mapofcontents:
___
## Java DataOutputStream Class
Класс **DataOutputStream** позволяет приложению записывать примитивные типы данных Java в выходной поток машинно-независимым способом. Приложение Java обычно использует поток вывода данных для записи данных, которые впоследствии могут быть прочитаны потоком ввода данных.

### Java DataOutputStream class declaration
```java
public class DataOutputStream extends FilterOutputStream implements DataOutput
```

**CODE EXAMPLE**
```java
public class DataOutputStreamDemo {
    public static void main(String[] args) throws IOException {
        DataOutputStream dataOutputStream =
                new DataOutputStream(new FileOutputStream(
"/home/proselyte/Programming/Projects/Proselyte/JavaCore/resources/inputFile.txt"));
        dataOutputStream.writeUTF("This is test message.");

        DataInputStream dataInputStream =
                new DataInputStream(new FileInputStream(
"/home/proselyte/Programming/Projects/Proselyte/JavaCore/resources/inputFile.txt"));

        while (dataInputStream.available() != 0) {
            String message = dataInputStream.readUTF();
            System.out.println(message);
        }
    }
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Input Output]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-dataoutputstream-class)
- [Proselyte](https://proselyte.net/tutorials/java-core/files-io/data-output-stream/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/DataOutputStream.html)
