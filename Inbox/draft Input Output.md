2022120720:07
___
Date: 07-12-2022 | 20:07
Tags: #code #java #io
mapofcontents:
___
## IO/NIO Ввод-Вывод

### Потоковый ввод-вывод _(java.io)_
* **InputStream** - читать байты
* **OutputStream** - писать байты
* **Reader** - читать символы
* **Writer** - писать символы

### InputStream
* `int read()` -> -1 или 0..255
* `int read(byte[] b)` -> кол-во прочитанных байт
* `int read(byte[] b, int offset, int length)` -> кол-во прочитанных байт
* `long skip(long n)` -> кол-во пропущенных байт
* `int available()` -> кол-во байт доступных без блокировки
* `void mark(int readlimit)`
* `void reset()`
* `boolean markSupported()`
* Java 9 и выше
* `byte[] readAllBytes()` -> все содержимое. Java 9+
* `byte[] readNBytes(int len)` -> не больше len байт. Java 11+
* `byte[] readNBytes(byte[] b, int offset, int length)` -> кол-во прочитанных байт до ошибки или конца потока. Java 11+
* `void skipNBytes(long n)` -> пропускает n байт
* `long transferTo(OutputStream out)` -> перекачивает в выходной поток. Java 9+
* `static InpytStream nullInputStream()` -> пустой поток. Java 11+

### Реализации InputStream
* **FileInputStream** - файл
* **ByteArrayInputStream** - массив байт
* **BufferedInputStream** - буферизированная обертка
* **DataInputStream** - структурированные двоичные данные
* **PipedInputStream** - плохой, негодный
* **ZipFile.getInputStream(ZipEntry)** - файл из зипа (без распаковки!)
* **Process.getInputStream()** - стандартный вывод процесса
* **URL.openStream()** - содержимое по URL(http/https/ftp/file/etc)
* ....

_Пример InputStream_  
_Вычитать все в массив заданного размера_
```java
class Main {
    static void readFully(InputStream is, byte[] b) throws IOException {
        int offset = 0;
        while (offset < b.length) {
            int count = is.read(b, offset, b.length-offset);
            if (count == -1) {
                throw new IOException("Stream has less than " + b.length + " bytes");
            }
            offset += count;
        }
    }

    public static void main(String[] args) throws IOException {
      byte[] b = new byte[100];
      readFully(System.in, b);
      System.out.println(Arrays.toString(b));
    }
}
```
_URL (HTTP-клиент для бедных)_
```java
class Main {
  public static void main(String[] args) {
    InputStream is = new URL("http://www.google.com").openStream();
    System.out.println(new String(is.readAllBytes(), StandardCharsets.UTF_8));
  }
}
```
_HttpClient (Java 11) - гибко_
```java
class Main {
  public static void main(String[] args) {
    HttpClient client = HttpClient.newHttpClient();
    HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("http://google.com/"))
            .build();
    client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
            .thenApply(HttpResponse::body)
            .thenAccept(System.out::println)
            .join();
  }
}
```
### Закрываем явно  
_Пример:_
```java
/* НЕ правильный подход*/
class Main {
  public static void main(String[] args) {
    InputStream f = new FileInputStream("/etc/passwd");
    int b = f.read();
    f.close();
  }
}
/* Правильный подход */
class Main {
  public static void main(String[] args) {
    InputStream f = new FileInputStream("/etc/passwd");
    try {
      int b = f.read();
    } finally {
        f.close();
    }
  }
}
/* Лучший подход */
class Main {
  public static void main(String[] args) {
    try(InputStream in = new FileInputStream("/etc/passwd");
        OutputStream out = new FileOutputStream("/etc/passwd.bak")) {
        in.transferTo(out);
    }
  }
}
```
### OutputStream
* `void write(int b)` -> пишет 8 младших бит
* `void write(byte[] b)` -> пишет весь массив (если не упал, то все записал)
* `void write(byte[]b, int offset, int length)` -> пишет часть массива
* `void flush()`
* `void close()`
* `static OutputStream nullOutputStream()` -> пишет сколько влезет (Java 11)

### Реализации OutputStream
* **FileOutputStream** - файл
* **ByteArrayOutputStream** - массив байт
* **BufferedOutputStream** - буферизированная обертка
* **PrintStream** - для удобства печати (System.out)
* **DataOutputStream** - структурированные двоичные данные
* **PipedOutputStream** - плохой (не использовать)
* **Process.getOutputStream()** - стандартный ввод процесса
* **URL.openConnection().getOutputStream()** - писать в URL (HTTP POST)
* ....

_Пример OutputStream_
```java
class Main {
  public static void main(String[] args) {
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    baos.write(0x01);
    baos.write(0x02);
    baos.write(0x03);
    byte[] result = baos.toByteArray();
    System.out.println(Arrays.toString(result));    // [1, 2, 3]
  }
}
```
### Reader
* `int read()` -> -1 или 0..0xFFFF
* `int read(char[] cb)` -> кол-во прочитанных символов
* `int read(char[] cb, int offset, int length)` -> кол-во прочитанных символов
* `long skip(long n)` -> кол-во пропущенных символов
* `boolean ready()` -> можно ли прочитать хоть что-то без блокировки
* `void mark(int readlimit)`
* `void reset()`
* `boolean markSupported()`
* `long transferTo(Write out)` - Java 10
* `static Reader nullReader()` - Java 11

### Реализации Reader
* **InputStreamReader** - InputStream + charset
* **FileReader** - осторожно
  * Java 11: **FileReader(file, charset)**
  * Java 18: **UTF-8 by default**
* **StringReader**
* **BufferedReader extends Reader**
  * String readLine()
  * Stream< String >lines()
* ....

### Writer
* `void write(int b)` -> пишет 8 младших бит
* `void write(char[] cb)` -> пишет весь массив (если не упал, то все записал)
* `void write(char[]cb, int offset, int length)` -> пишет часть массива
* `void write(String str)` - пишет всю строку, если не упал
* `void write(String str, int offset, int length)` - пишет часть строки
* `void flush()`
* `void close()`
* `static Write nullWriter()` -> пишет, сколько влезет (Java 11)

### Реализации Writer
* **OutputStreamReader** - OutputStream + charset
* **FileWriter** - осторожно
  * Java 11: **FileWriter(file, charset)**
  * Java 18: **UTF-8 by default**
* **StringWriter**
* **BufferedWriter**

### _java.io.File_

* ~~new File(parentDirectory + "/" + fileName);~~~~
* ~~new File(parentDirectory + File.separator + fileName);~~
* new File(parentDirectory, fileName);
* **Mетоды**
* `getName()/getPath()/getParent()/getParentFile()`
* `getAbsoluteFile()/getAbsolutePath()`
* `getCanonicalFile()/getCanonocalPath()`
* `exists()/canRead()/canWrite()/canExecute()/length()`
* `isDirectory()/isFile()/isHidden()`
* `createNewFile()/mkdir()/mkdirs()`
* `renameTo()/delete()`
* `list([filter])/listFiles([filter])`

### _java.nio.file.Path/Paths/Files_

* `Path p = Paths.get("/etc/passwd");`
* `Path p = Paths.get(parentDirectory, fileName);`
* `Path p = Paths.get("foo", "bar", "baz");`
* **Java 11+**
* `Path p = Path.of("/etc/passwd");`
* `Path p = Path.of(parentDirectory, fileName);`
* `Path p = Path.of("foo", "bar", "baz");`
* **Path (Comparable)**
  * `getFileName()`
  * `getParent()`
  * `getRoot()`
  * `resolve(Path/String), resolveSibling(Path/String)`
  * `isAbsolute(), toAbsolutePath()`
  * `startsWith(Path/String), endsWith(Path/String)`
  * `getName(int), getNameCount(), iterator()`
  * `toString(), toFile(), toUri()`
  * `register(WatchService, WatchEvent.Kind...)`
* **Files**
  * `copy()`
  * `move()`
  * `delete(), deleteIfExist()`
  * `createFile(), createDirectory(), createDirectories()`
  * `createLink(), createSimbolicLink()`
  * `createTempFile(), createTempDirectory()`
  * `readAllBytes(), readAllLines()`
  * `write(byte[], iterable<String>)`
  * `lines, list, walk` -> Stream
  * `walkFileTree()`
  * `exist, size, getAttribute, isDirectory, isRegularFile, isSimbolicLink...`
  * `newBufferedReader, newInputStream, newOutputStream, newByteChannel`
  
_Примеры:_

```java
import java.io.File;

/* Пример плохого кода */
public class Listing {
  public static void main(String[] args) {
    var etcDirectory = new File("/etc");
    for (String fileName : etcDirectory.list()) {
      System.out.println("File: " + fileName);
    }
  }
} 
```

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

/* Пример допустимого кода */
public class Listing {
  public static void main(String[] args) throws IOException {
    var etcDirectory = Paths.get("/etc");
    try (var stream = Files.list(etcDirectory)) {
      stream.forEach(fileName -> System.out.println("File: " + fileName));
    }
  }
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
