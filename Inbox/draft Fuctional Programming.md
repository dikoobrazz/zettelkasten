2022120720:08
___
Date: 07-12-2022 | 20:08
Tags: #code #java #functional
mapofcontents:
___
## Elements of functional programming

### Функциональный интерфейс
* Интерфейс (не абстрактный класс), содержащий единственный абстрактный метод
* Может быть аннотирован как `@FunctionalInterface`

### Функциональное выражение в Java
* Отображается на функциональный интерфейс
* Не имеет состояния
* Конкретный тип устанавливается по контексту
* Либо лямбда-выражение `((a, b) -> a+b)` либо ссылка на метод `(Integer::sum)`
* Не гарантирует `identity`
* Не гарантирует `getClass() identity`

### Лямбда выражение
* Аргументы:
    * `(Type1 name1, Type2 name2)`
    * `(name1, name2)`
    * `Name`
* Стрелочка `->`
* Тело:
    * Выражение `System.out.println()`
    * Блок `{ System.out.println(); }`
* Void-compatible (void SAM-single abstract method):
    * Блок: каждый return не содержит выражения
    * Выражение: допустимое в утверждении (вызов метода, присваивание, печать и т.д.)
* Value-compatible (non-void SAM-single abstract method):
    * Блок: каждый return содержит выражение и нормальное завершение невозможно
    * Выражение: имеет значение не void
    
### Примеры лямбда-выражений
```java
// Пример лямбды
public class Main {
    public static void main(String[] args) {
        Runnable r = () -> System.out.println("Hello");
        r.run();
        Runnable rn = (args.length > 0 ?
                () -> System.out.println("Hello " + args[0] + "!") :
                () -> System.out.println("Hello World!"));
    }
}
```
```java
// Пример лямбды
public class Main {
    static void run(Runnable r) {
        r.run();
    }

    public static void main(String[] args) {
        run(() -> System.out.println("Hello world!"));
    }
}
```
```java
// Пример лямбды
public class Main {
    public static void main(String[] args) {
        Object x = (Runnable) (() -> System.out.println("Hello!"));
    }
}
```
```java
class Main {
    public static void main(String[] args) {
        IntSupplier x = () -> 5;
        IntSupplier x2 = () -> System.out.println(5); // Error. Value-compatible
        
        Runnable y1 = () -> 5; // Error. Void-compatible
        Runnable y2 = ()-> System.out.println(5);
    }
}
```

### Захват (Capture) значений
* **Локальная переменная** - должна быть effectively final и инициализирована к месту использования лямбды, берется значение этой переменной.
* **Поле текущего класса** - захватывается this, значение из поля читается при выполнении.
* **Статическое поле** - ничего не захватывается, значение читается при выполнении.
* **Closure (Замыкание)** - лямбда-выражение с захваченными переменными.

### Примеры захвата значений (замыканий)
```java
class Main {
    int field = 10;
    static int sField = 15;
    
    void test() {
        int var = 5;
        Rnnable r1 = () -> System.out.println(var);
        Rnnable r2 = () -> System.out.println(field);
        Rnnable r3 = () -> System.out.println(sField);
        field = 5;
        sField = 5;
        r1.run(); // 5
        r2.run(); // 5
        r3.run(); // 5
    }
}
```
### Ссылка на метод (method reference)
* Статический метод: 
    * `IntBinaryOperator sum = Integer::sum;`
* Не статический метод (первый параметр SAM-метода превращается в квалификатор): 
    * `Function<String, String> trimmer = String::trim; // res = trimmer.apply("  hello  ");`
* Не статический метод привязанный к экземпляру(instance-bound):
    * `Predicate<String> isFoo = "foo"::equals;`
    * `Consumer<Object> printer = System.out::println;`
* Конструктор:
    * `Supplier<List<String>> listFactory = ArrayList::new;`
* Конструирование массива:
    * `IntFunction<String[]> arrayFactory = String[]::new;`
    
_Пример_
```java
class Main {
    public static void main(String[] args) {
        Predicate<String> pred = "Java"::equals;
        System.out.println(pred.test("Java"));      // true
        System.out.println(pred.test("Kotlin"));    // false
        
        Consumer<Object> methodRefPrinter = System.out::println;
        Consumer<Object> lambdaPrinter = obj -> System.out.println(obj);
        System.setOut(null);
        methodRefPrinter.accept("hello");   // hello
        lambdaPrinter.accept("hello");      // Error. NullPointerException
    }
}
```
### Примитивы функционального программирования

_Примеры_

```java
import java.util.Objects;
import java.util.function.BiFunction;

class Main {
    // Привязка аргумента (bind)
    static <A, B, C> Function<B, C> bind(BiFunction<A, B, C> fn, A a) {
        Objects.requireNonNull(fn);
        return b -> fn.apply(a, b);
    }
    
    // Карирование (curry)
    static <A, B, C> Function<A, Function<B, C>> curry(Bifunction<A, B, C> fn) {
        return a -> b -> fn.apply(a, b);
    }

    public static void main(String[] args) {
        Function<Integer, Integer> inc = bind(Integer::sum, 1);
        System.out.println(inc.apply(10));  // 11

        System.out.println(curry(Integer::sum).apply(5).apply(6));  // 11
    }
}
```

### Optional< T >
* Либо значение (не null), либо его отсутствие
* Optional.of(obj) создает Optional с переданным значением
* Optional.ofNullable(obj) если значение не null - создается Optional со значением,  
  если значение null - создается пустой Optional 
* Optional.empty() создает пустой Optional

_Пример:_
```java
class Main {
    static Optional<Integer> toInteger(String opt ){
        try {
            return Optional.of(Integer.valueOf(opt));
        } catch (NumberFormatException ex) {
            return Optional.empty();
        }
    }
}
```
### Методы Optional _(не все, часто используемые)_
* `filter(Predicate T)`
    * `boolean isFooEqualsBar = Optional.of("foo").filter("bar"::equals).isPresent();`
* `<U> map(Function<T, U>)`
    * `String trimmed = Optional.of("  foo  ").map(String::trim).get();`
* `<U> flatMap(Function<T, Optional<U>>)`
    * `Integer num = Optional.of("1234").flatMap(x -> toInteger(x)).orElse(-1);`
* `orElseGet(Supplier<T>)`
    * `Double random = Optional.<Double>empty().orElseGet(Math::random);`
* `<X extends Throwable> orElseThrow(Supplier<T>) throws X`
    * `Double random = Optional.<Double>empty().orElseThrow(Exception::new);`
* `or(Supplier<? extends Optional<? extends T>>)`
    * `getOneOptional().or(() -> getAnotherOptional());`
    
_Пример:_
```java
interface User {
    String name();
    boolean isActive();
    void updateCarma(int delta);
}
interface UserRepository {
    Optional<User> findUser(String name);
}

class Main {
    void increaseUserCarma(UserRepository repository, String name) {
        repository.findUser(name)
                .filter(User::isActive)
                .orElseThrow(() -> new IllegalArgumentException("No such user: " + name))
                .updateCarma(1);
    }
}
```

## Stream API 

<!--more-->

_Специализации:_
* _java.util.stream.Stream_
* _java.util.stream.IntStream_
* _java.util.stream.LongStream_
* _java.util.stream.DoubleStream_

### Stream
* Поток однотипных данных для однотипной обработки
* Источник создает ленивый стрим
* Промежуточные операции описывают рецепт обработки, но ничего не делают
* Вся работа производится при вызове терминальной операции
* Может потребить не все элементы
* Может быть бесконечным или завершиться за конечное время

_Пример:_
```java
class Main {
    public static void main(String[] args) {
        source.stream()
                .map(x -> x.squash())
                .filter(x -> x.getColor() != YELLOW)
                .forEach(System.out::println);
    }
}
```

### Некоторые источники Stream
* `Stream.empty()`
* `Stream.of(x, y, z)`
* `Stream.ofNullable(x)`
* `Stream.generate(Math::random)`
* `Stream.iterate(val, x -> x + 1)`
* `Stream.iterate(0, x -> x < 100, x -> x + 1)` - Java 9
* `collection.stream()`
* `Arrays.stream(array)` - int/long/double/Object
* `Random.ints()/longs()/doubles()`
* `String.chars()`
* `Pattern.aplitAsStream()`
* ....

### Промежуточные операции _(intermediate)_
* `map() mapToInt/mapToLong/mapToDouble/mapToObj`
* `filter()`
* `flatMap() flatMapToInt/flatMapToLong/flatMapToDouble`
* `mapMulti mapMultiToInt/mapMultiToLong/mapMultiToDouble` - Java 16
* `distinct()`
* `sorted()`
* `limit()`
* `skip()`
* `peek()`
* `takeWhile()` - Java 9
* `dropWhile()` - Java 9
* `boxed asLongStream/asDoubleStream` - примитивы

### Терминальные операции _(terminal)_
* `count()`
* `findFirst()/findAny -> Optional`
* `anyMatch()/noneMatch/allMatch`
* `forEach()/forEachOrdered`
* `max()/min -> Otional`
* `reduce()`
* `collect()`
* `toArray()`
* `toList()` - Java 16
* `sum average/summaryStatistics` - примитивы

_Примеры:_

```java
import java.util.stream.Collectors;

class Main {
  // не правильная реализация  
  public List<String> processList(List<String> list) {
    List<String> result = new ArrayList<>();
    list.stream()
            .map(String::trim)
            .filter(s -> !s.isEmpty())
            .forEach(result::add);
    return result;
  }

  // правильная реализация
  public List<String> processList(List<String> list) {
    return list.stream()
            .map(String::trim)
            .filter(s -> !s.isEmpty())
            .collect(Collectors.toList());
  }
}
```
```java
class Main {
  public static void main(String[] args) {
    List<List<String>> listOfLists = List.of(
            List.of("foo", "bar", "baz"),
            List.of("Java", "Kotlin", "Groovy"),
            List.of("Hello", "World")
    );
    System.out.println(
            listOfLists.stream()
            .flatMap(List::stream)
            .anyMatch("Java"::equals)
    );
  }
}
```
```java
class Main {
  // reduce
  static BigInteger factorial(int n) {
    return IntStream.rangeClosed(1, n)
            .mapToObj(BigInteger::valueOf)
            .reduce(BigInteger.ONE, BigInteger::multiply);
  }
}
```
**Don't over reduce!**
* `reduce(0, Integer::sum) -> mapToInt(x -> x).sum()`
* `reduce(Integer::max) -> max()`
* `reduce("", String::concat) -> collect(Collectors.joining())`

### Collector
* Рецепт терминальной операции (способ комбинирования элементов в единое целое)
* Предопределенные коллекторы в классе Collectors
* Некоторые коллекторы можно комбинировать
* Свой коллектор с нуля (Collector.of)

### Collectors
* `toList()` - ArrayList (Не гарантированно)
* `toSet()` - HashSet (Не гарантированно)
* `toCollection(supplier)`
* `toMap(keyMapper, valueMapper[, merger[, mapSupplier]])`
* `joining([separator[, prefix, suffix]])`
* `groupingBy(classifier[[, mapSupplier], downstream])`
* `partitioningBy(predicate[, downstream])`
* `reducing/counting/mapping/minBy/maxBy`
* `averagingInt/averagingLong/averagingDouble`
* `summingInt/summingLong/summingDouble`

_Задача. Сгруппировать строки по длине_  
_Пример 1_
```java
class Main {
  // Задача. Сгруппировать строки по длине
  static Map<Integer, List<String>> stringsByLength(List<String> list) {
    return list.stream().collect(Collectors.groupingBy(String::length));
  }

  public static void main(String[] args) {
    stringsByLength(Arrays.asList("a", "bb", "c", "dd", "eee"));
    // {1=[a, c], 2=[bb, dd], 3=[eee]}
  }
}
```
_Пример 2_
```java
class Main {
  static Map<Integer, String> stringsByLength(List<String> list) {
    return list.stream().collect(Collectors.groupingBy(String::length, Collectors.joining("+")));
  }

  public static void main(String[] args) {
    stringsByLength(Arrays.asList("a", "bb", "c", "dd", "eee"));
    // {1=a+c, 2=bb+dd, 3=eee}
  }
}
```
_Пример 3_
```java
class Main {
  static Map<Integer, Map<Character, List<String>>> strByLenAndFirstLetter(List<String> list) {
    return list.stream()
            .collect(Collectors.groupingBy(String::length,
                    Collectors.groupingBy(s -> a.charAt(0))));
  }

  public static void main(String[] args) {
    strByLenAndFirstLetter(Arrays.asList("a", "b", "aa", "ab", "ba"));
    // {1={a=[a], b=[b]}, 2={a=[aa, ab], b=[ba]}
  }
}
```
_Пример с объектами 1:_
```java
interface User { 
    String name(); 
}
interface Department {
    String title();
    User chief();
    Stream<User> users();
}
interface Company { 
    Stream<Department> departments(); 
}

class Main {
    // Список отделов по начальнику
    static Map<User, List<Department>> departmentByChief(Company company) {
        return company
                .departments()
                .collect(Collectors.groupingBy(Department::chief));
    }
    
    // Список названий отделов по начальнику
    static Map<User, List<String>> departmentNamesByChief(Company company) {
        return company
                .departments()
                .collect(Collectors.groupingBy(Department::chief,
                        Collectors.mapping(Department::title, toList())));
    }
    
    // Найти подчиненных для каждого начальника
    // Java 9
    static Map<User, Set<User>> supervisors(Company company) {
        return company
                .departments()
                .collect(Collectors.groupingBy(Department::chief, 
                        Collectors.flatMapping(Department::users, toSet())));
    }
}
```
_Пример с объектами 2:_
```java
interface Book {
    String getCategory();
    int getPrice();
}
class Main {
    /* Найти самую дешёвую книгу в каждой категории */
  
    // !! Не верное решение!!
    static Map<String, Integer> getLowBookPriseByCategory(List<Book> list) {
        return list.stream()
                .collect(Collectors.groupingBy(Book::getCategory,
                        Collectors.minBy(Comparator.comparingInt(Book::getPrice))));
    }

    // Верное решение
    static Map<String, Integer> getLowBookPriseByCategory(List<Book> list) {
      return list.stream()
              .collect(Collectors.toMap(
                      Book::getCategory, Book::getPrice,
                      BinaryOperator.minBy(Comparator.naturalOrder())
              ));
    }
}
```


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
