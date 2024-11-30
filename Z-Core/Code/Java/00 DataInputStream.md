2022121117:45
___
Date: 11-12-2022 | 17:45
Tags: #code #java #io 
mapofcontents:
___
## Java DataInputStream Class 
Класс **DataInputStream** позволяет приложению считывать примитивные данные из входного потока машинно-независимым способом. Приложение Java обычно использует поток вывода данных для записи данных, которые впоследствии могут быть прочитаны потоком ввода данных.

### Java DataInputStream class declaration
```java
public class DataInputStream extends FilterInputStream implements DataInput
```

**CODE EXAMPLE**
```java
public class DataStreamExample {  
	public static void main(String[] args) throws IOException {  
		InputStream input = new FileInputStream("testout.txt");  
		DataInputStream inst = new DataInputStream(input);  
		int count = input.available();  
		byte[] ary = new byte[count];  
		inst.read(ary);  
		for (byte bt : ary) {  
			char k = (char) bt;  
			System.out.print(k+"-");  
		}  
	}  
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Input Output]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-datainputstream-class)
- [Proselyte](https://proselyte.net/tutorials/java-core/files-io/data-input-stream/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/DataInputStream.html)
