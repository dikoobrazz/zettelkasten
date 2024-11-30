2022121115:07
___
Date: 11-12-2022 | 15:07
Tags: #code #java #io 
mapofcontents:
___
## Java FileInputStream Class
Класс **FileInputStream** получает входные байты из файла. Он используется для чтения данных, ориентированных на байты (потоки необработанных байтов), таких как данные изображения, аудио, видео и т. д. Вы также можете читать данные символьного потока. Но для чтения потоков символов рекомендуется использовать класс [[00 FileReader|FileReader]].

### Java FileInputStream class declaration
```java
public class FileInputStream extends InputStream
```

**CODE EXAMPLE**
```java
public class InputMain {
	public static void main(String[] args) {
	 = null;
	try (FileInputStream input = new FileInputStream("data/info.txt")){
		int code = input.read();
		System.out.println(code + " char = " + (char)code);
		byte[] arr = new byte[16];
		int numberBytes = input.read(arr);
		System.out.println("numberBytes = " + numberBytes);
		System.out.println(Arrays.toString(arr));

		// read all characters
		int i=0;    
		while((i = fin.read())!=-1){    
			System.out.print((char)i);
		}
	} catch (FileNotFoundException e) {
		e.printStackTrace();
	} catch (IOException e) {
		e.printStackTrace();
	} finally {
	
	}
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 FileReader]]
- [[ml Java Input Output]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-fileinputstream-class)
- [Proselyte](https://proselyte.net/tutorials/java-core/files-io/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/FileInputStream.html)
