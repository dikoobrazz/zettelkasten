2022121519:26
___
Date: 15-12-2022 | 19:26
Tags: #code #book #java #sort 
mapofcontents:
___
## Быстрая сортировка. Разбиение Хоара
Выбрать в последовательности опорный элемент. Этот элемент выбирается произвольным образом (обычно - это первый элемент последовательности)
В последовательности сравнить все остальные элементы с опорным и переставить их в
последовательности так, чтобы разбить ее на две непрерывных области, следующих друг за
другом: "элементы меньшие опорного" и "больше опорного". 
Рекурсивно вызвать пункт 1 сначала для подпоследовательности где хранятся элементы меньше опорного, после для подпоследовательности с элементами больше опорного. Условие прекращение рекурсии это размер просматриваемой подпоследовательности равный 1.

**QUICK SORT HAARA**
```java
public static void quickSort(int[] array) {  
    quickSort(array, 0, array.length - 1);  
}  
  
public static void quickSort(int[] array, int lo, int hi) {  
    // Условие выхода из рекурсии  
    if (lo >= hi) {  
        return;  
    }  
    int h = breakPartition(array, lo, hi);  
    quickSort(array, lo, h - 1);  
    quickSort(array, h + 1, hi);  
}  
  
// Алгоритм разбиения Хаара  
private static int breakPartition(int[] array, int lo, int hi) {  
    int i = lo + 1;  
    int supportElement = array[lo];  
    int j = hi;  
  
    while (true) {  
        while (i < hi && array[i] < supportElement) i++;  
        while (array[j] > supportElement) j--;  
        if (i >= j) break;  
        swap(array, i++, j--);  
    }  
    swap(array, lo, j);  
    return j;  
}  
  
// Helper  
public static void swap(int[] array, int i, int j) {  
    int temp = array[i];  
    array[i] = array[j];  
    array[j] = temp;  
}
```

## Быстрая сортировка. Разбиение Ломуто
В качестве опорного элемента выбирается первый элемент последовательности (supportElement). Объявляется две переменных для хранения индексов (в дальнейшем `i` и `j`). Значение `j` равно первому индексу в подпоследовательности.
Начиная от начала последовательности проводиться поиск элемента для которого
`sequince[i]<supportElement`. Если такой элемент найден, то увеличиваем значение `j` на одну
единицу, производим обмен элементов которые стоят на `i` и `j` индексе.
Проводим обмен первого элемента последовательности и элемента на индексе `j`. Возвращаем
значение индекса `j` и заканчиваем текущее разбиение.

**QUICK SORT LOMUTO**
```java

```


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Sort Array]]
- [[ml Arrays Collections]]
- [[JAVA Решение практических задач.]]

------
**Links** (Внешние ссылки)
- [Book Code Github](https://github.com/PacktPublishing/Java-Coding-Problems/blob/master/Chapter05/P99_SortArray/src/modern/challenge/ArraySorts.java)
- [Quick Sort Hoara Video](https://www.youtube.com/watch?v=6CkfbqzN0N4)
- [Quick Sort Lomuto Video](https://www.youtube.com/watch?v=G6v8ocbDSoo&t=28s)
