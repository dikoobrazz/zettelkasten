2022121515:34
___
Date: 15-12-2022 | 15:34
Tags: #code #book #java #sort 
mapofcontents:
___
## Сортировка вставками
Алгоритм сортировки вставками основан на простом процессе управления. Он начинается со второго элемента и сравнивает его с предыдущим элементом. Если предыдущий элемент больше текущего, то алгоритм меняет местами элементы. Этот процесс продолжается до тех пор, пока предыдущий элемент не станет меньше текущего элемента.

Временная сложность алгоритма составляет: в лучшем случае — О(n), в среднем случае — О(n2), в худшем случае — О(n2).

**INSERT SORT**
```java
public static void insertionSort(int[] arr) {  
    int n = arr.length;  
    for (int i = 1; i < n; i++) {  
  
        int key = arr[i];  
        int j = i - 1;  
  
        while (j >= 0 && arr[j] > key) {  
            arr[j + 1] = arr[j];  
            j = j - 1;  
        }  
        arr[j + 1] = key;  
    }  
}
```

**INSERT SORT OBJECT (COMPARATOR)**
```java
public static <T> void insertionSortWithComparator(T[] arr, Comparator<? super T> c) {  
    int n = arr.length;  
    for (int i = 1; i < n; i++) {  
  
        T key = arr[i];  
        int j = i - 1;  
  
        while (j >= 0 && c.compare(arr[j], key) > 0){  
            arr[j + 1] = arr[j];  
            j = j - 1;  
        }  
        arr[j + 1] = key;  
    }  
}
```

> [!note] NOTE
> Этот алгоритм бывает быстрым для малых и во многом отсортированных массивов. Кроме того, он хорошо работает при добавлении в массив новых элементов. Он также является очень эффективным по потребляемой памяти, т.к. по ней перемещается один-единственный элемент.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[../Code/Java/00 Array|Array]]
- [[ml Sort Array]]
- [[ml Arrays Collections]]
- [[JAVA Решение практических задач.]]

------
**Links** (Внешние ссылки)
- [Book Code Github](https://github.com/PacktPublishing/Java-Coding-Problems/blob/master/Chapter05/P99_SortArray/src/modern/challenge/ArraySorts.java)
