2022121114:48
___
Date: 11-12-2022 | 14:48
Tags: #code #java #io 
mapofcontents:
___
## Java FileOutputStream Class
**FileOutputStream** — это поток вывода, используемый для записи данных в файл. Если вам нужно записать примитивные значения в файл, используйте класс FileOutputStream. Вы можете записывать как байтовые, так и символьные данные с помощью класса FileOutputStream. Но для символьных данных предпочтительнее использовать [[00 FileWriter|FileWriter]], чем FileOutputStream.

### FileOutputStream class declaration
```java
public class FileOutputStream extends OutputStream
```

**CODE EXAMPLE**
```java
public class OutMain {
	public static void main(String[] args) {
		try (FileOutputStream output = new FileOutputStream("data/out.txt", true)) {
			output.write(48);
			String s = "Welcome to javaTpoint.";
			byte[] value = s.getBytes();
			output.write(value);
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 FileWriter]]
- [[ml Java Input Output]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-fileoutputstream-class)
- [Proselyte](https://proselyte.net/tutorials/java-core/files-io/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/FileOutputStream.html)
