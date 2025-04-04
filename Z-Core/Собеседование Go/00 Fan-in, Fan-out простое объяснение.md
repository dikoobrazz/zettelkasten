2025040221:21
___
Date: 02-04-2025 | 21:21
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Fan-in, Fan-out простое объяснение

### 1. Fan-out: Распараллеливание работы

В паттерне **fan-out** мы создаём несколько горутин, каждая из которых будет работать над своей частью задачи. То есть, мы "распараллеливаем" работу между несколькими воркерами.
##### Пример: Распараллеливаем обработку данных

Предположим, у нас есть список чисел, и мы хотим их обработать в несколько горутин:
```go
package main

import (
	"fmt"
	"sync"
)

func worker(id int, ch <-chan int, wg *sync.WaitGroup) {
	defer wg.Done() // Уменьшаем счётчик после завершения работы

	for num := range ch {
		fmt.Printf("Работник %d обрабатывает число %d\n", id, num)
	}
}

func main() {
	var wg sync.WaitGroup
	ch := make(chan int, 10) // Канал для передачи чисел

	// Запускаем несколько горутин для обработки данных
	for i := 1; i <= 3; i++ {
		wg.Add(1)
		go worker(i, ch, &wg)
	}

	// Отправляем данные в канал
	for i := 1; i <= 10; i++ {
		ch <- i
	}

	// Закрываем канал и ждём завершения всех воркеров
	close(ch)
	wg.Wait()
}
```
### Объяснение:

- Мы создаём несколько воркеров (горутин), каждый из которых будет обрабатывать данные из канала.
- Канал используется для передачи чисел, и каждый воркер будет обрабатывать эти числа, как только они поступят.
- Это распределяет нагрузку между несколькими горутинами, что ускоряет выполнение, если задача требует много времени на обработку.
##### Вывод:
```text
Работник 1 обрабатывает число 1
Работник 2 обрабатывает число 2
Работник 3 обрабатывает число 3
Работник 1 обрабатывает число 4
Работник 2 обрабатывает число 5
...
```

---
### 2. Fan-in: Собираем результаты из нескольких горутин

В паттерне **fan-in** мы собираем результаты работы нескольких горутин в один канал. Это полезно, когда мы выполняем параллельные вычисления и хотим собрать их в одном месте.
##### Пример: Собираем результаты из нескольких каналов

Предположим, у нас несколько горутин, каждая из которых генерирует числа, и мы хотим собрать их все в одном канале:
```go
package main

import (
	"fmt"
	"sync"
)

func generateNumbers(id int, ch chan<- int, wg *sync.WaitGroup) {
	defer wg.Done() // Уменьшаем счётчик после завершения работы

	for i := 1; i <= 5; i++ {
		ch <- i * id
	}
}

func main() {
	var wg sync.WaitGroup
	ch := make(chan int, 15) // Канал для сбора данных

	// Запускаем несколько горутин для генерации чисел
	for i := 1; i <= 3; i++ {
		wg.Add(1)
		go generateNumbers(i, ch, &wg)
	}

	// Используем горутину для сбора результатов
	go func() {
		wg.Wait() // Ждём завершения всех горутин
		close(ch) // Закрываем канал
	}()

	// Читаем из канала все собранные данные
	for num := range ch {
		fmt.Println(num)
	}
}
```

### Объяснение:

- Мы создаём несколько горутин, каждая из которых генерирует последовательность чисел.
- Результаты каждой горутины передаются в один общий канал `ch`.
- В главной горутине мы читаем все данные из канала и выводим их на экран.
##### Вывод:
```text
1
2
3
4
5
2
4
6
8
10
3
6
9
12
15
```

**Что важно:**

- Канал `ch` служит как объединяющий поток для данных, сгенерированных разными горутинами.
- Мы закрываем канал только после того, как все горутины завершат работу, что гарантирует, что мы не будем пытаться читать из закрытого канала.

---
### 3. Как сочетать fan-out и fan-in?

Ты можешь комбинировать оба паттерна: распараллеливать обработку и собирать результаты в одном месте. Например, сначала ты распараллеливаешь обработку данных, а затем собираешь их в один канал.
##### Пример: Распараллеливаем обработку и собираем результаты
```go
package main

import (
	"fmt"
	"sync"
)

func worker(id int, ch <-chan int, result chan<- int, wg *sync.WaitGroup) {
	defer wg.Done()

	for num := range ch {
		// Эмулируем обработку данных
		result <- num * id
	}
}

func main() {
	var wg sync.WaitGroup
	inputCh := make(chan int, 10)  // Канал для входных данных
	resultCh := make(chan int, 10) // Канал для результатов

	// Запускаем несколько горутин для обработки
	for i := 1; i <= 3; i++ {
		wg.Add(1)
		go worker(i, inputCh, resultCh, &wg)
	}

	// Заполняем входной канал данными
	for i := 1; i <= 10; i++ {
		inputCh <- i
	}

	close(inputCh) // Закрываем входной канал

	// Ждём завершения горутин
	go func() {
		wg.Wait()
		close(resultCh) // Закрываем канал с результатами
	}()

	// Собираем результаты из канала
	for result := range resultCh {
		fmt.Println(result)
	}
}
```

### Объяснение:

- В этом примере мы создаём два канала: один для входных данных (`inputCh`), другой для результатов (`resultCh`).
- Мы используем несколько воркеров, которые берут данные из `inputCh`, обрабатывают их и отправляют результаты в `resultCh`.
- Результаты собираются в основном потоке, пока горутины выполняют обработку.
##### Вывод:
```text
1
2
3
4
5
6
7
8
9
10
2
4
6
8
10
...
```

---
### **Подводные камни и советы:**

1. **Закрытие каналов:**
    
    - Важно правильно закрывать каналы. Мы закрываем канал только после того, как все горутины завершат работу.
    - Закрытие канала в промежутке может вызвать ошибки или блокировки, если другие горутины ещё пытаются писать в канал.
        
2. **Использование `WaitGroup`:**
    
    - `WaitGroup` помогает синхронизировать завершение всех горутин, чтобы не закрыть канал раньше времени.
        
3. **Использование буферизированных каналов:**
    
    - Если ты работаешь с большим количеством данных, буферизированные каналы могут помочь уменьшить блокировки, позволяя горутинам работать быстрее.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
