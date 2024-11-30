2022121515:22
___
Date: 15-12-2022 | 15:22
Tags: #code #book #java #sort
mapofcontents: 
___
## Сортировка пузырьком
Сортировка пузырьком — это простой алгоритм, который выталкивает элементы массива вверх. Это означает, что он обходит массив в цикле несколько раз и меняет местами соседние элементы, если они расположены в неправильном порядке.

Временная сложность алгоритма составляет: в лучшем случае — О(n), в среднем случае — О(n2), 
в худшем случае — О(n2).

**BUBBLE SORT PRIMITIVE**
```java
public static void bubbleSort(int[] arr) {  
    int n = arr.length;  
    for (int i = 0; i < n - 1; i++) {  
        for (int j = 0; j < n - i - 1; j++) {  
            if (arr[j] > arr[j + 1]) {  
                int temp = arr[j];  
                arr[j] = arr[j + 1];  
                arr[j + 1] = temp;  
            }  
        }  
    }  
}
```

**BUBBLE SORT PRIMITIVE OPTIMIZED**
```java
public static void bubbleSortOptimized(int[] arr) {  
    int n = arr.length;  
    while (n != 0) {  
        int swap = 0;  
        for (int i = 1; i < n; i++) {  
            if (arr[i - 1] > arr[i]){  
                int temp = arr[i];  
                arr[i] = arr[i - 1];  
                arr[i - 1] = temp;  
  
                swap = i;  
            }  
        }  
        n = swap;  
    }  
}
```

**BUBBLE SORT OBJECTS (COMPORATOR)**
```java
public static <T> void bubbleSortOptimizedWithComparator(
				T[] arr, Comparator<? super T> c) {  
    int n = arr.length;  
    while (n != 0) {  
        int swap = 0;  
        for (int i = 1; i < n; i++) {  
            if (c.compare(arr[i - 1], arr[i]) > 0){  
                T temp = arr[i];  
                arr[i] = arr[i - 1];  
                arr[i - 1] = temp;  
  
                swap = i;  
            }  
        }  
        n = swap;  
    }  
}
```

> [!note] NOTE
> Сортировка пузырьком работает быстро, когда массив почти отсортирован. Кроме того, она хорошо подходит для сортировки "кроликов" (больших элементов, которые находятся близко к началу массива) и "черепах" (малых элементов, которые находятся близко к концу массива). Но в целом этот алгоритм является медленным.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[../Code/Java/00 Array|Array]]
- [[ml Sort Array]]
- [[ml Arrays Collections]]
- [[JAVA Решение практических задач.]]

------
**Links** (Внешние ссылки)
- [Book Code Github](https://github.com/PacktPublishing/Java-Coding-Problems/blob/master/Chapter05/P99_SortArray/src/modern/challenge/ArraySorts.java)
