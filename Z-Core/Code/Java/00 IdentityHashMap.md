2022120719:03
___
Date: 07-12-2022 | 19:03
Tags: #code #java #collections 
mapofcontents:
___
## Java IdentityHashMap Class
Этот класс реализует интерфейс [[00 Map|Map]] с хэш-таблицей, используя равенство ссылок вместо равенства объектов при сравнении ключей (и значений). Другими словами, в IdentityHashMap два ключа `k1` и `k2` считаются равными тогда и только тогда, когда `(k1==k2)`. В обычных реализациях [[00 Map|Map]] (например, [[00 HashMap|HashMap]]) два ключа `k1` и `k2` считаются равными тогда и только тогда, когда `(k1==null ? k2==null : k1.equals(k2))`.

> [!warning] Caution!
> Этот класс не является реализацией Map общего назначения! Хотя этот класс реализует интерфейс Map, он намеренно нарушает общий контракт Map, который предписывает использование метода [[00 ContractHashEquals|equals]] при сравнении объектов. Этот класс предназначен для использования только в тех редких случаях, когда требуется семантика ссылочного равенства.

**CODE EXAMPLE**
```java
public class IdentityHashMapDemo {
    public static void main(String[] args) {
        IdentityHashMap<integer, string=""> identityHashMap = new IdentityHashMap<>();

        System.out.println("Adding elements into IdentityHashMap...");
        identityHashMap.put(3, "Proselyte");
        identityHashMap.put(2, "AsyaSmile");
        identityHashMap.put(5, "Konstantin");
        identityHashMap.put(1, "Ivan");
        identityHashMap.put(4, "Peter");

        System.out.println("Initial identityHashMap content:");
        Set set = identityHashMap.entrySet();

        for (Object element : set) {
            Map.Entry mapEntry = (Map.Entry) element;
            System.out.println("ID: " + mapEntry.getKey() + ", Name: " + 
            mapEntry.getValue());
        }

        System.out.println("\n================================\n");

        System.out.println("Modifying Proselyte...");
        String name = identityHashMap.get(3);
        name += " Changed";
        identityHashMap.put(3, name);

        System.out.println("\n================================\n");

        System.out.println("Initial identityHashMap content:");
        set = identityHashMap.entrySet();

        for (Object element : set) {
            Map.Entry mapEntry = (Map.Entry) element;
            System.out.println("ID: " + mapEntry.getKey() + ", Name: " + 
            mapEntry.getValue());
        }

        System.out.println("\n================================\n");
    }
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[00 Map]]
- [[00 HashMap]]
- [[00 ContractHashEquals]]
- [[ml Java Collections API]]

------
**Links** (Внешние ссылки)
- [Baeldung](https://www.baeldung.com/java-identityhashmap)
- [Proselyte](https://proselyte.net/tutorials/java-core/collections-framework/identity-hashmap/)
- [Oracle Documentation](https://docs.oracle.com/javase/7/docs/api/java/util/IdentityHashMap.html)
