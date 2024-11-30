2022121115:19
___
Date: 11-12-2022 | 15:19
Tags: #code #java #io 
mapofcontents:
___
## Java BufferedInputStream Class
Класс **BufferedInputStream** используется для чтения информации из потока. Он внутренне использует буферный механизм для повышения производительности.

Важные моменты в BufferedInputStream: 
- Когда байты из потока пропускаются или считываются, внутренний буфер автоматически перезаполняется из содержащегося в нем входного потока по многим байтам за раз. 
- При создании BufferedInputStream создается [[00 Array|массив]] внутренних буферов.

### Java BufferedInputStream class declaration
```java
public class BufferedInputStream extends FilterInputStream
```

**CODE EXAMPLE**
```java
public class BufferedInputStreamExample{    
	public static void main(String args[]){    
		try (BufferedInputStream bin=new BufferedInputStream(
			new FileInputStream("testout.txt")
		)){        
			int i;   
			while((i = bin.read()) != -1){    
				System.out.print((char)i);    
			}    
		}catch(Exception e){
			System.out.println(e);
		}    
	}    
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Input Output]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-bufferedinputstream-class)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/BufferedInputStream.html)
