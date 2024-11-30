2022121116:25
___
Date: 11-12-2022 | 16:25
Tags: #code #java #io
mapofcontents:
___
## Java ByteArrayInputStream Class
**ByteArrayInputStream** состоит из двух слов: **ByteArray** и **InputStream**. Как следует из названия, его можно использовать для чтения [[00 Array|массива]] байтов в качестве входного потока. Класс Java **ByteArrayInputStream** содержит внутренний буфер, который используется для чтения массива байтов в виде потока. В этом потоке данные считываются из массива байтов. Буфер **ByteArrayInputStream** автоматически увеличивается в соответствии с данными.

### Java ByteArrayInputStream class declaration
```java
public class ByteArrayInputStream extends InputStream
```

**CODE EXAMPLE**
```java
public class ByteArrayInputStreamDemo {
    public static void main(String[] args) throws IOException {
        ByteArrayOutputStream byteArrayOutputStream =
                new ByteArrayOutputStream(20);
        
        System.out.println("Enter the string (up to 12 elements): ");
        while (byteArrayOutputStream.size() < 13) {
            byteArrayOutputStream.write(System.in.read());
        }

        byte[] byteArray = byteArrayOutputStream.toByteArray();
        System.out.println("Byte Array: ");
        for(byte b: byteArray){
            System.out.print((char) b + " ");
        }

        System.out.println("\n=======================\n");

        int i;

        ByteArrayInputStream byteArrayInputStream = 
            new ByteArrayInputStream(byteArray);

        System.out.println(
	        "Now we will convert all elements of array to lower case: "
        );

        while ((i = byteArrayInputStream.read())!= -1){
            System.out.print(Character.toLowerCase((char)i));
        }
        byteArrayInputStream.close();
    }
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- 

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-bytearrayinputstream-class)
- [Proselyte](https://proselyte.net/tutorials/java-core/files-io/byte-array-input-stream/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/io/ByteArrayInputStream.html)
