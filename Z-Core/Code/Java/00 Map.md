2022120517:15
___
Date: 05-12-2022 | 17:15
Tags: #code #java #collections 
mapofcontents:
___
## Java Map Interface

> [!notes] INFO 
> Map содержит значения на основе ключа, т.е. пары ключ-значение. 
> Каждая пара ключ-значение называется `entry`. 
> Карта содержит уникальные ключи. 
> Map полезен, если нужно искать, обновлять или удалять элементы на основе ключа.


![[_resources/Map.png]]


## Иерархия Java Map

Есть два интерфейса для реализации Map в java: 
- Map 
- SortedMap 
Три класса: 
- [[00 HashMap|HashMap]] 
- [[00 LinkedHashMap|LinkedHashMap]] 
- [[00 TreeMap|TreeMap]]
- [[00 EnumMap|EnumMap]]

**HashMap** - реализация Map, но она не поддерживает никакого порядка.
**LinkedHashMap** - реализация Map. Наследует класс HashMap. Поддерживает порядок вставки.
**TreeMap** - реализация Map и SortedMap. Поддерживает восходящий порядок вставки.

> [!notes] INFO
> Map не допускает дублирования ключей, но могут быть повторяющиеся значения. 
> [[00 HashMap|HashMap]] и [[00 LinkedHashMap|LinkedHashMap]] допускают нулевые ключи и значения. 
> [[00 TreeMap|Treemap]] не допускает никаких нулевых ключей или значений.

### Interface Map (methods)

- `int size();`
* `boolean isEmpty();`
* `boolean containsKey(Object key);`
* `boolean containsValue(Object value);`
* `V get(Object key);`
* `V put(K key, V value);`
* `V remove(Object key);`
* `void putAll(Map<? extends K, ? extends V> m);`
* `void clear();`
* `Set<K> setKey();`
* `Collections<V> values();`
* `Set<Map.entry<K, V>> entrySet();`
* `V getOrDefault(Object key, V defaultvalue);`
* `V putIfAbsent(K key, V value);`
* `boolean remove(Object key, Object value);`
* `boolean replace(K key, V oldValue, V newValue);`
* `V replace(K key, V value);`

>[!notes] INFO!
>Map.Entry Interface
> 	Entry — это субинтерфейс Map. Таким образом, мы будем обращаться к нему по имени Map.Entry. Он возвращает коллекцию-представление Map, элементы которой относятся к этому классу. Он предоставляет методы для получения ключа и значения.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 HashMap]]
- [[00 TreeMap]]
- [[00 LinkedHashMap]]
- [[00 EnumMap]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Javatpoint](https://www.javatpoint.com/java-map)
- [Oracle Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)
