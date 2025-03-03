2025030315:44
___
Date: 03-03-2025 | 15:44
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Что есть в пакете sync и для чего он нужен

> Пакет **`sync`** в Go предоставляет набор примитивов синхронизации для управления конкурентностью в программах, где используются горутины. Он нужен для координации работы горутин, защиты общих ресурсов от одновременного доступа и предотвращения гонок данных (race conditions).

---
### Что есть в пакете sync?

#### 1. Mutex (Взаимное исключение)

- **Что это**: `sync.Mutex` — мьютекс для защиты критических секций.
- **Методы**:  
    - `Lock()`: Блокирует мьютекс.
    - `Unlock()`: Разблокирует.
- **Для чего**: Защищает общие данные от одновременного изменения несколькими горутинами.
##### Пример
```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var mu sync.Mutex
    counter := 0
    var wg sync.WaitGroup

    for i := 0; i < 100; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            mu.Lock()
            counter++
            mu.Unlock()
        }()
    }
    wg.Wait()
    fmt.Println("Счетчик:", counter) // Счетчик: 100
}
```

- **Зачем**: Без `Mutex` `counter` мог бы быть меньше 100 из-за гонок.

---
#### 2. RWMutex (Мьютекс для чтения и записи)

- **Что это**: `sync.RWMutex` — мьютекс с разделяемыми блокировками для чтения и эксклюзивной для записи.
- **Методы**:  
    - `RLock()`/`RUnlock()`: Для чтения (много горутин одновременно). 
    - `Lock()`/`Unlock()`: Для записи (только одна горутина).
- **Для чего**: Оптимизация доступа, когда чтение частое, а запись редкая.
##### Пример
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var rwmu sync.RWMutex
    data := 0
    var wg sync.WaitGroup

    // Читатели
    for i := 1; i <= 3; i++ {
        wg.Add(1)
        go func(id int) {
            defer wg.Done()
            rwmu.RLock()
            fmt.Printf("Читатель %d видит: %d\n", id, data)
            time.Sleep(100 * time.Millisecond)
            rwmu.RUnlock()
        }(i)
    }

    // Писатель
    wg.Add(1)
    go func() {
        defer wg.Done()
        rwmu.Lock()
        data = 42
        fmt.Println("Писатель записал:", data)
        rwmu.Unlock()
    }()

    wg.Wait()
}
```

- **Зачем**: Много читателей работают одновременно, писатель блокирует всех.

---
#### 3. WaitGroup

- **Что это**: `sync.WaitGroup` — счетчик для ожидания завершения группы горутин.
- **Методы**:  
    - `Add(n)`: Увеличивает счетчик на nnn.
    - `Done()`: Уменьшает счетчик на 1.
    - `Wait()`: Блокирует, пока счетчик не станет 0.
- **Для чего**: Синхронизация завершения задач.
##### Пример
```go
package main

import (
    "fmt"
    "sync"
)

func task(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Задача %d\n", id)
}

func main() {
    var wg sync.WaitGroup
    for i := 1; i <= 3; i++ {
        wg.Add(1)
        go task(i, &wg)
    }
    wg.Wait()
    fmt.Println("Все задачи завершены")
}
```

- **Зачем**: Гарантирует, что `main` дождется всех горутин.

---
#### 4. Cond (Условная переменная)

- **Что это**: `sync.Cond` — примитив для ожидания и сигнализации о событиях.
- **Методы**:  
    - `Wait()`: Блокирует горутину до сигнала.
    - `Signal()`: Будит одну ждущую горутину.
    - `Broadcast()`: Будит все ждущие.
- **Для чего**: Синхронизация по условиям.
##### Пример
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var mu sync.Mutex
    cond := sync.NewCond(&mu)
    ready := false
    var wg sync.WaitGroup

    wg.Add(1)
    go func() {
        defer wg.Done()
        mu.Lock()
        fmt.Println("Жду готовности")
        for !ready {
            cond.Wait() // Ждет сигнала
        }
        fmt.Println("Готово!")
        mu.Unlock()
    }()

    time.Sleep(1 * time.Second)
    mu.Lock()
    ready = true
    cond.Signal() // Будим
    mu.Unlock()

    wg.Wait()
}
```

- **Зачем**: Ожидание события (например, готовности ресурса).

---
#### 5. Once

- **Что это**: `sync.Once` — гарантирует однократное выполнение функции.
- **Методы**:  
    - `Do(f)`: Выполняет *f* один раз, даже при множественных вызовах.
- **Для чего**: Ленивая инициализация.
##### Пример
```go
package main

import (
    "fmt"
    "sync"
)

var once sync.Once

func initialize() {
    fmt.Println("Инициализация")
}

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 3; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            once.Do(initialize)
        }()
    }
    wg.Wait()
}
```

- **Зачем**: Только одна горутина выполнит `initialize`.

---
#### 6. Pool

- **Что это**: `sync.Pool` — пул объектов для повторного использования.
- **Методы**:  
    - `Get()`: Извлекает объект из пула (или создает новый).
    - `Put(x)`: Возвращает объект в пул.
- **Для чего**: Уменьшение аллокаций и нагрузки на GC.
##### Пример
```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    pool := sync.Pool{
        New: func() interface{} {
            return &struct{ Value int }{}
        },
    }

    obj1 := pool.Get().(*struct{ Value int })
    obj1.Value = 42
    fmt.Println("obj1:", obj1.Value)
    pool.Put(obj1)

    obj2 := pool.Get().(*struct{ Value int })
    fmt.Println("obj2:", obj2.Value) // Может быть 42, если переиспользован
}
```

- **Зачем**: Кэширование объектов (например, буферов).
  
---
#### 7. Map

- **Что это**: `sync.Map` — конкурентно-безопасная карта.
- **Методы**:  
    - `Store(key, value)`: Устанавливает значение.
    - `Load(key)`: Получает значение.
    - `Delete(key)`: Удаляет ключ.
- **Для чего**: Замена map с мьютексом для простых случаев.
##### Пример
```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var m sync.Map
    var wg sync.WaitGroup

    for i := 0; i < 3; i++ {
        wg.Add(1)
        go func(id int) {
            defer wg.Done()
            m.Store(id, fmt.Sprintf("value%d", id))
        }(i)
    }

    wg.Wait()
    if val, ok := m.Load(1); ok {
        fmt.Println("Значение:", val) // Значение: value1
    }
}
```

- **Зачем**: Безопасное хранение пар ключ-значение.

---
### Для чего нужен sync?

Пакет sync нужен для:

- **Синхронизации**: Координация выполнения горутин (`WaitGroup`, `Cond`).
- **Защиты данных**: Предотвращение гонок (`Mutex`, `RWMutex`, `Map`).
- **Оптимизации**: Уменьшение накладных расходов (`Once`, `Pool`).
  
Он дополняет каналы, предоставляя низкоуровневые инструменты для случаев, где каналы избыточны или неудобны.

---
### Итог

В sync есть:

- `Mutex`, `RWMutex` — для защиты данных.
- `WaitGroup` — для ожидания горутин.
- `Cond` — для событий.
- `Once` — для однократного выполнения.
- `Pool` — для повторного использования объектов.
- `Map` — для конкурентной карты.

Используйте их, когда нужно управлять конкурентностью без каналов или с большей точностью.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
