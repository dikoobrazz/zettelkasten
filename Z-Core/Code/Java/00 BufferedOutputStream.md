2022121115:35
___
Date: 11-12-2022 | 15:35
Tags: #code #java #io 
mapofcontents:
___
## Java BufferedOutputStream Class
Класс **BufferedOutputStream** используется для буферизации выходного потока. Он внутренне использует буфер для хранения данных. Это добавляет большей эффективности, чем запись данных непосредственно в поток. Таким образом, это делает производительность быстрой. Для добавления буфера в **OutputStream** используйте класс **BufferedOutputStream**. 

==Cинтаксис добавления буфера в **OutputStream**:==
```java
OutputStream os= new BufferedOutputStream(new FileOutputStream("testout.txt"));
```

### Java BufferedOutputStream class declaration
```java
public class BufferedOutputStream extends FilterOutputStream
```

**CODE EXAMPLE**
```java
public class BufferedOutputStreamExample{    
	public static void main(String args[])throws Exception{    
		FileOutputStream fout=new FileOutputStream("testout.txt");    
		BufferedOutputStream bout=new BufferedOutputStream(fout);    
		String s="Welcome to java";    
		byte b[]=s.getBytes();    
		bout.write(b);    
		bout.flush();    
		bout.close();    
		fout.close();    
		System.out.println("success");    
	}    
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Input Output]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-bufferedoutputstream-class)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/BufferedOutputStream.html)
