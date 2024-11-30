2022120819:56
___
Date: 08-12-2022 | 19:56
Tags: #code #java #exception 
mapofcontents: 
___
## Java Custom Exception
В Java мы можем создавать собственные исключения, являющиеся производными классами **class Exception**. Создание нашего собственного исключения известно как **custom exception** или 
**user-defined exception**. В основном пользовательские исключения Java используются для настройки исключения в соответствии с потребностями пользователя.

> [!note] NOTE
> Чтобы создать пользовательское исключение, нам нужно расширить класс Exception, принадлежащий пакету java.lang.

> [!note] NOTE
> Нам нужно написать конструктор, который принимает String в качестве сообщения об ошибке, и оно передается конструктору родительского класса.

**CODE EXAMPLE**
```java
public class WrongFileNameException extends Exception {  
	public WrongFileNameException(String errorMessage) {  
		super(errorMessage);  
	}  
}
```

Используя пользовательское исключение, мы можем получить собственное исключение и сообщение. Здесь мы передали строку конструктору суперкласса, т.е. класса Exception, который можно получить с помощью метода getMessage() для созданного нами объекта.

### Условия создания собственных исключений:
- Исключение должно быть наследником класса **Throwable**
- Для создания "проверяемого" исключения, мы должны наследоваться от класса **Exception**.
- Для создания "непроверяемого" исключения, мы должны наследоваться от класса **RuntimeException**

**CODE EXAMPLE**
```java
public class NegativeWidthException extends Exception{
    private int width;
    public NegativeWidthException(int width){
        this.width = width;
    }
    public int getWidth() {
        return width;
    }
}

public class Square {
    private int width;

    public Square(int width) {
        this.width = width;
    }

    public int getWidth() {
        return width;
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public int calculateArea(int width) throws NegativeWidthException {
        if (width >= 0) {
            return width * width;
        }else {
            throw new NegativeWidthException(width);
        }
    }
}

public class Main {
    public static void main(String[] args)throws NegativeWidthException{
        Square square = new Square(-1);

        System.out.println("Square width: " + square.getWidth());

        System.out.printf("Calculating area...\n");
        System.out.println("Area: " + square.calculateArea(square.getWidth()));
    }
}
```


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Throw exception]]
- [[00 Throws keyword]]
- [[00 Exception Basics]]
- [[00 Exception Types]]
- [[ml Java Exceptions]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/custom-exception)
- [Proselyte](https://proselyte.net/tutorials/java-core/exceptions/)
