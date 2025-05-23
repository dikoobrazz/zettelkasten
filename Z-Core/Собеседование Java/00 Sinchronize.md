2025041415:44
___
Date: 14-04-2025 | 15:44
Tags: #
mapofcontents: [[zero-links|OO Links]]
___

В Java ключевое слово **`synchronized`** — это механизм для управления доступом к общим ресурсам в многопоточной среде, обеспечивающий **синхронизацию** потоков. Оно предотвращает одновременное выполнение критических секций кода несколькими потоками, что помогает избежать **гонок данных** (race conditions) и обеспечивает **потокобезопасность**. Давайте подробно разберём, что такое `synchronized`, его основное назначение, как оно работает, зачем нужно, и приведём примеры с нюансами.

---

### Что такое `synchronized`?

**`synchronized`** — это ключевое слово в Java, которое используется для синхронизации доступа к методам или блокам кода. Оно гарантирует, что только **один поток** может выполнять синхронизированный код в определённый момент времени, блокируя остальные потоки, пока текущий не завершит выполнение.

#### Основные характеристики:
1. **Взаимное исключение (Mutual Exclusion)**:
   - `synchronized` создаёт **монитор** (lock), который позволяет только одному потоку выполнять синхронизированный код. Другие потоки ждут, пока монитор не освободится.
2. **Видимость изменений**:
   - При входе и выходе из синхронизированного блока JVM синхронизирует локальные кэши потоков с основной памятью, обеспечивая видимость изменений, сделанных одним потоком, для других.
3. **Атомарность**:
   - Гарантирует, что набор операций в синхронизированном блоке выполняется как единое целое, без вмешательства других потоков.
4. **Happens-before**:
   - Устанавливает связь happens-before: все действия до выхода из `synchronized` блока видны потоку, который затем получает тот же монитор.

#### Два способа использования:
1. **Синхронизированный метод**:
   ```java
   public synchronized void myMethod() {
       // Критическая секция
   }
   ```
   - Блокирует весь метод, используя монитор объекта (`this` для нестатических методов) или класса (для `static` методов).
2. **Синхронизированный блок**:
   ```java
   synchronized (lockObject) {
       // Критическая секция
   }
   ```
   - Блокирует только указанный блок кода, используя монитор объекта `lockObject`.

---

### Основное назначение

`synchronized` используется для:
1. **Предотвращения гонок данных**:
   - Когда несколько потоков изменяют общие данные (например, счётчик), `synchronized` гарантирует, что изменения происходят последовательно.
2. **Обеспечения потокобезопасности**:
   - Защищает критические секции кода, где доступ к ресурсам (память, файлы, базы данных) должен быть ограничен одним потоком.
3. **Координации потоков**:
   - Позволяет потокам работать с общими ресурсами в определённом порядке, избегая конфликтов.
4. **Гарантии видимости**:
   - Убедиться, что изменения, сделанные одним потоком, видны другим.

---

### Зачем нужно?

`synchronized` нужно в многопоточных приложениях, где:
- **Общие ресурсы**:
  - Несколько потоков обращаются к одной переменной, коллекции или объекту (например, банковский счёт, список заказов).
- **Критические секции**:
  - Код изменяет состояние, которое должно быть согласованным (например, инкремент счётчика, запись в файл).
- **Координация**:
  - Потоки должны ждать друг друга (например, продюсер ждёт, пока потребитель освободит буфер).
- **Избежание ошибок**:
  - Без синхронизации возможны ошибки, такие как потерянные обновления, некорректные данные или непредсказуемое поведение.

**Когда НЕ использовать `synchronized`**:
- Если достаточно `volatile` (например, для флага без сложной логики).
- Если нужны более гибкие механизмы (например, `ReentrantLock` для таймаутов).
- Для атомарных операций, где лучше подходят классы из `java.util.concurrent.atomic` (например, `AtomicInteger`).
- Если высокая конкуренция снижает производительность (рассмотрите неблокирующие структуры, такие как `ConcurrentHashMap`).

---

### Как работает `synchronized`?

Чтобы понять работу `synchronized`, нужно рассмотреть **монитор** и **модель памяти Java** (Java Memory Model, JMM):

1. **Монитор**:
   - Каждый объект в Java может выступать как монитор (lock).
   - Когда поток входит в `synchronized` блок или метод, он **захватывает монитор** объекта (или класса для `static` методов).
   - Другие потоки, пытающиеся захватить тот же монитор, **блокируются** и ждут в очереди.
   - После завершения критической секции поток **освобождает монитор**, позволяя другому потоку продолжить.

2. **Синхронизация памяти**:
   - При входе в `synchronized` блок поток обновляет локальный кэш из основной памяти.
   - При выходе изменения записываются в основную память.
   - Это гарантирует, что все потоки видят актуальные данные.

3. **Блокировка**:
   - `synchronized` использует **встроенные блокировки** (intrinsic locks) JVM, которые реализованы на уровне операционной системы.
   - Блокировка может быть **реентерабельной** (reentrant): поток может многократно захватывать один и тот же монитор (например, при рекурсии).

---

### Примеры использования

#### Пример 1: Счётчик с синхронизацией
Без `synchronized` многопоточный доступ к счётчику приводит к гонкам данных.

```java
public class Counter {
    private int count = 0;

    // Без синхронизации (ошибка)
    public void increment() {
        count++; // Не атомарно: чтение, увеличение, запись
    }

    public int getCount() {
        return count;
    }
}
```

Теперь добавим `synchronized`:

```java
public class SynchronizedCounter {
    private int count = 0;

    // Синхронизированный метод
    public synchronized void increment() {
        count++;
    }

    // Альтернатива: синхронизированный блок
    public void incrementWithBlock() {
        synchronized (this) {
            count++;
        }
    }

    public synchronized int getCount() {
        return count;
    }

    public static void main(String[] args) throws InterruptedException {
        SynchronizedCounter counter = new SynchronizedCounter();
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Итоговый счётчик: " + counter.getCount());
    }
}
```

**Объяснение**:
- Без `synchronized` операция `count++` не атомарна, и результат может быть меньше ожидаемого 2000 (например, 1998).
- С `synchronized` только один поток может выполнять `increment()` в момент времени, гарантируя корректный результат.
- **Вывод**:
  ```
  Итоговый счётчик: 2000
  ```

#### Пример 2: Синхронизация по объекту
Если нужно синхронизировать доступ к общему ресурсу, используйте блок `synchronized`.

```java
public class SharedResource {
    private final List<String> items = new ArrayList<>();
    private final Object lock = new Object(); // Отдельный объект для блокировки

    public void addItem(String item) {
        synchronized (lock) {
            items.add(item);
            System.out.println(Thread.currentThread().getName() + " добавил: " + item);
        }
    }

    public List<String> getItems() {
        synchronized (lock) {
            return new ArrayList<>(items); // Защитная копия
        }
    }

    public static void main(String[] args) {
        SharedResource resource = new SharedResource();
        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                resource.addItem("Элемент " + i);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        };

        Thread t1 = new Thread(task, "Поток-1");
        Thread t2 = new Thread(task, "Поток-2");
        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println("Итоговый список: " + resource.getItems());
    }
}
```

**Объяснение**:
- `lock` — отдельный объект для синхронизации, чтобы избежать блокировки всего экземпляра.
- Каждый поток добавляет элементы по очереди, избегая одновременного доступа к `items`.
- Защитная копия в `getItems()` предотвращает изменение списка извне.
- **Вывод** (порядок может варьироваться):
  ```
  Поток-1 добавил: Элемент 0
  Поток-2 добавил: Элемент 0
  Поток-1 добавил: Элемент 1
  ...
  Итоговый список: [Элемент 0, Элемент 0, Элемент 1, Элемент 1, ...]
  ```

#### Пример 3: Статическая синхронизация
Для защиты статических данных используется `synchronized` на уровне класса.

```java
public class StaticSynchronized {
    private static int count = 0;

    public static synchronized void increment() {
        count++;
    }

    // Эквивалентно:
    public static void incrementWithBlock() {
        synchronized (StaticSynchronized.class) {
            count++;
        }
    }

    public static int getCount() {
        synchronized (StaticSynchronized.class) {
            return count;
        }
    }
}
```

**Объяснение**:
- `synchronized` на статическом методе блокирует монитор **класса** (`StaticSynchronized.class`), а не объекта.
- Это защищает доступ к статическому полю `count` от всех потоков.

---

### Сравнение с другими механизмами

| **Характеристика**       | **`synchronized`**                        | **`volatile`**                             | **`ReentrantLock`**                       |
|--------------------------|------------------------------------------|--------------------------------------------|------------------------------------------|
| **Видимость**            | Гарантирует                              | Гарантирует                                | Гарантирует                              |
| **Атомарность**          | Да                                       | Нет                                        | Да                                       |
| **Гибкость**             | Ограниченная (нет таймаутов)             | Только для переменных                      | Высокая (таймауты, условия)              |
| **Блокировка**           | Встроенный монитор                       | Нет                                        | Явная блокировка                         |
| **Производительность**   | Средняя (зависит от конкуренции)         | Высокая (без блокировок)                   | Высокая (гибкие настройки)               |
| **Использование**        | Критические секции, простая синхронизация| Флаги, видимость                           | Сложная синхронизация                    |

---

### Нюансы и Best Practices

1. **Когда использовать `synchronized`**:
   - Для защиты критических секций с общими ресурсами (например, коллекции, счётчики).
   - Когда нужна простая и надёжная синхронизация без сложной логики.
   - Пример: Защита банковского счёта:
     ```java
     public synchronized void transfer(Account to, int amount) {
         balance -= amount;
         to.deposit(amount);
     }
     ```

2. **Когда НЕ использовать**:
   - Если достаточно `volatile` (например, для флага `isRunning`).
   - Для высокой конкуренции, где блокировки снижают производительность (рассмотрите `ConcurrentHashMap` или `Atomic`).
   - Если нужны таймауты или условия (используйте `ReentrantLock`).

3. **Производительность**:
   - `synchronized` может вызывать **ожидание** (contention), если много потоков борются за монитор.
   - Решение: Минимизируйте размер критической секции:
     ```java
     public void doWork() {
         // Несинхронизированный код
         synchronized (lock) {
             // Только критическая секция
         }
         // Несинхронизированный код
     }
     ```

4. **Монитор**:
   - Используйте **отдельный объект** для блокировки, чтобы избежать ненужной блокировки всего экземпляра:
     ```java
     private final Object lock = new Object();
     ```

5. **Deadlocks**:
   - Неправильное использование `synchronized` может привести к **взаимоблокировкам**:
     ```java
     synchronized (lock1) {
         synchronized (lock2) { ... }
     }
     // В другом потоке:
     synchronized (lock2) {
         synchronized (lock1) { ... }
     }
     ```
   - Решение: Упорядочите захват блокировок, используйте таймауты с `ReentrantLock`.

6. **Статическая vs инстанс-синхронизация**:
   - Статическая синхронизация (`synchronized static`) блокирует весь класс, что может снизить производительность.
   - Используйте с осторожностью, если данные не глобальные.

7. **Альтернативы**:
   - **java.util.concurrent**:
     - `AtomicInteger` для счётчиков.
     - `ConcurrentHashMap` для коллекций.
     - `ReentrantLock` для гибкой синхронизации.
     - `ExecutorService` для управления потоками.
   - Пример с `AtomicInteger` вместо `synchronized`:
     ```java
     private AtomicInteger count = new AtomicInteger(0);
     public void increment() {
         count.incrementAndGet();
     }
     ```

8. **Spring и `synchronized`**:
   - В Spring избегайте `synchronized` в бинах, так как они часто создаются как singletons, и блокировки могут стать узким местом.
   - Используйте `@Transactional` для синхронизации на уровне базы или `@Lock` из Spring Integration.

---

### Итог

- **Что такое `synchronized`**:
  - Ключевое слово для синхронизации доступа к критическим секциям, обеспечивающее взаимное исключение и видимость изменений.
- **Основное назначение**:
  - Предотвращать гонки данных, обеспечивать потокобезопасность, координировать потоки.
- **Зачем нужно**:
  - Защищать общие ресурсы (счётчики, коллекции, файлы) в многопоточных приложениях.
  - Избежать ошибок, таких как потерянные обновления или несогласованные данные.
- **Примеры**:
  - Синхронизированный счётчик, защита списка, статическая синхронизация.
- **Нюансы**:
  - Может снижать производительность при высокой конкуренции.
  - Требует осторожности, чтобы избежать взаимоблокировок.
  - Для сложных сценариев рассмотрите `java.util.concurrent`.
- **Best Practices**:
  - Минимизируйте критические секции.
  - Используйте отдельные объекты для блокировки.
  - Рассмотрите альтернативы для высокой нагрузки.

Если хотите углубиться в конкретный аспект (например, сравнение `synchronized` с `ReentrantLock`, примеры в Spring или разбор deadlock-сценариев), уточните, и я распишу ещё подробнее!


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
