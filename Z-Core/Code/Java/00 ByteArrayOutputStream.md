2022121116:12
___
Date: 11-12-2022 | 16:12
Tags: #code #java #io 
mapofcontents:
___
## Java ByteArrayOutputStream Class
Этот класс реализует поток вывода, в котором данные записываются в [[00 Array|массив]] байтов. Буфер автоматически увеличивается по мере записи в него данных. Данные можно получить с помощью toByteArray() и toString(). Закрытие ByteArrayOutputStream не имеет никакого эффекта. Методы этого класса можно вызывать после закрытия потока без создания исключения IOException.

> [!note] 
> **ByteArrayOutputStream** содержит копию данных и может пересылать ее в несколько потоков.

![[_resources/java-byte-array-output-stream-class1.png]]

### Java ByteArrayOutputStream class declaration
```java
public class ByteArrayOutputStream extends OutputStream
```

**CODE EXAMPLE**
```java
public class DataStreamExample {  
	public static void main(String args[])throws Exception{    
		FileOutputStream fout1 = new FileOutputStream("f1.txt");    
        FileOutputStream fout2 = new FileOutputStream("f2.txt");    
		ByteArrayOutputStream bout = new ByteArrayOutputStream();    
		bout.write(65);    
		bout.writeTo(fout1);    
		bout.writeTo(fout2);    
		bout.flush();    
		bout.close();//has no effect   
		System.out.println("Success...");    
	}    
}
```

```java
public class ByteArrayOutputStreamDemo {
    public static void main(String[] args) throws IOException{
        ByteArrayOutputStream byteArrayOutputStream =
                new ByteArrayOutputStream(20);

        System.out.println("Enter the string: ");
        while (byteArrayOutputStream.size() != 13){
            byteArrayOutputStream.write(System.in.read());
        }

        byte[] byteArray = byteArrayOutputStream.toByteArray();
        System.out.println("byteArray: ");
        for(byte b: byteArray){
            System.out.print((char)b);
        }

        System.out.println("\n=======================\n");

        int i;

        ByteArrayInputStream byteArrayInputStream =
                new ByteArrayInputStream(byteArray);

        System.out.println("Now we will convert all characters to lower case:");

        while ((i = byteArrayInputStream.read())!= -1){
            System.out.print(Character.toLowerCase((char)i));
        }
        byteArrayInputStream.reset();
    }
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Java Input Output]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-bytearrayoutputstream-class)
- [Proselyte](https://proselyte.net/tutorials/java-core/files-io/byte-array-output-stream/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/ByteArrayOutputStream.html)
