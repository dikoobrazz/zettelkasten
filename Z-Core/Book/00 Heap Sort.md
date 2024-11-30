2022121517:16
___
Date: 15-12-2022 | 17:16
Tags: #code #book #java #sort 
mapofcontents: 
___
## Сортировка кучей
Алгоритм сортировки кучей (или кучевая сортировка) опирается на двоичную кучу (полное двоичное дерево).

Временная сложность алгоритма составляет: в лучшем случае — O(nlogN), в среднем случае — O(nlogN), в худшем случае — O(nlogN).

> [!info] INFO
> Сортировка элементов в возрастающем порядке может осуществляться посредством max-кучи (т.е. в которой родительский узел всегда больше или равен дочерним узлам), а в убывающем порядке - посредством min-кучи (т.е. в которой родительский узел всегда меньшеили равен дочерним узлам)

![[_resources/ArrayAsHeap.png]]


На первом шаге данный алгоритм использует массив, предоставленный для построения этой кучи и преобразования ее в max-кучу (куча представлена другим массивом). Поскольку эта куча является max-кучей, самым большим элементом будет корень кучи. На следующем шаге корень заменяется последним элементом из кучи, и размер кучи уменьшается на 1 (последний узел из кучи удаляется). Элементы, находящиеся в верхней части кучи, выводятся в отсортированном порядке. Последний шаг состоит из процедуры heapify (рекурсивного процесса, который строит кучу сверху вниз) и корня кучи (реконструирования max-кучи). Эти три шага повторяются до тех пор, пока размер кучи не превысит 1.

**HEAP SORT**
```java
public static void heapSort(int[] arr) {  
    int n = arr.length;  
  
    buildHeap(arr, n);  
  
    while (n > 1) {  
        swap(arr, 0, n-1);  
        n--;  
        heapify(arr, n, 0);  
    }  
}  
  
private static void buildHeap(int[] arr, int n) {  
    for (int i = arr.length/2; i >= 0; i--) {  
        heapify(arr, n, i);  
    }  
}  
  
private static void heapify(int[] arr, int n, int i) {  
    int left = i * 2 + 1;  
    int right = i * 2 + 2;  
    int greater;  
  
    if (left < n && arr[left] > arr[i]) {  
        greater = left;  
    } else {  
        greater = i;  
    }  
  
    if (right < n && arr[right] > arr[greater]) {  
        greater = right;  
    }  
  
    if (greater != i) {  
        swap(arr, i, greater);  
        heapify(arr, n, greater);  
    }  
}  
  
private static void swap(int[] arr, int x, int y) {  
    int temp = arr[x];  
    arr[x] = arr[y];  
    arr[y] = temp;  
}  
  
public static void main(String[] args) {  
    int[] numbers = {45,1,76,4,34,21,90,5,7,243};  
    heapSort(numbers);  
    System.out.println(Arrays.toString(numbers));  
}
```


> [!note] NOTE
> Сортировка кучи работает довольно быстро, но нестабильно. Например, сортировка массива, который уже отсортирован, может оставить его в другом порядке.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[../Code/Java/00 Array|Array]]
- [[ml Sort Array]]
- [[ml Arrays Collections]]
- [[JAVA Решение практических задач.]]

------
**Links** (Внешние ссылки)
- [Book Code Github](https://github.com/PacktPublishing/Java-Coding-Problems/blob/master/Chapter05/P99_SortArray/src/modern/challenge/ArraySorts.java)
- [Heap Sort Video](https://www.youtube.com/watch?v=DU1uG5310x0)
- [Java Heap Sort Video](https://www.youtube.com/watch?v=92yCSMwsz88)
