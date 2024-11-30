2022121515:42
___
Date: 15-12-2022 | 15:42
Tags: #code #book #java #sort 
mapofcontents:
___
## Сортировка подсчетом
Сортировка подсчетом начинается с вычисления минимального и максимального элементов в массиве. На основе вычисленных минимума и максимума алгоритм определяет новый массив, который будет нужен для подсчета неотсортированных элементов, используя элемент в качестве индекса. Кроме того, этот новый массив модифицируется так, чтобы каждый элемент в каждом индексе хранил сумму предыдущих количеств. Наконец, отсортированный массив получается из этого нового
массива.

Временная сложность алгоритма составляет: в лучшем случае — О(n + k),
в среднем случае — О(n + k), в худшем случае — О(n + k)

> [!info] !INFO
> k - число возможных значений в интервале
> n - число элементов, подлежащих сортировке
> Применим только для целых чисел
> Этот алгоритм очень быстрый!

**COUNTING SORT**
```java
public static void countingSort(int[] arr) {  
    int min = arr[0];  
    int max = arr[0];  
  
    for (int i = 1; i < arr.length; i++) {  
        if (arr[i] < min) min = arr[i];  
        else if (arr[i] > max) max = arr[i];  
    }  
  
    int[] counts = new int[max - min + 1];  
  
    for (int i = 0; i < arr.length; i++) {  
        counts[arr[i] - min]++;  
    }  
  
    int sortedIndex = 0;  
  
    for (int i = 0; i < counts.length; i++) {  
        while (counts[i] > 0) {  
            arr[sortedIndex++] = i + min;  
            counts[i]--;  
        }  
    }
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[../Code/Java/00 Array|Array]]
- [[ml Sort Array]]
- [[ml Arrays Collections]]
- [[JAVA Решение практических задач.]]


------
**Links** (Внешние ссылки)
- [Book Code Github](https://github.com/PacktPublishing/Java-Coding-Problems/blob/master/Chapter05/P99_SortArray/src/modern/challenge/ArraySorts.java)
- [Сортировка подсчетом](https://www.youtube.com/watch?v=oGJuM4XKFfs)
