## Hierarchy of Map - Classes
```mathematica
Map<K, V>  
    ▲  
    ├── HashMap<K, V>   (uses a hash table + linked list/binary tree for collision handling)  
    │  
    ├── LinkedHashMap<K, V>   (extends HashMap, maintains insertion order with a doubly linked list)  
    │  
    ├── TreeMap<K, V>   (implements NavigableMap, uses a Red-Black Tree for sorting keys)  
    │  
    ├── EnumMap<K extends Enum<K>, V>   (implements Map directly, optimized for enums, uses an array)  
    │  
    ├── WeakHashMap<K, V>   (extends AbstractMap, uses weak references for garbage collection)  
    │  
    ├── IdentityHashMap<K, V>   (extends AbstractMap, uses reference equality `==` instead of `.equals()`)  
    │  
    ├── Hashtable<K, V>   (implements Map directly, synchronized, legacy class)  
    │  
    ├── ConcurrentHashMap<K, V>   (implements ConcurrentMap, thread-safe, uses segment-based locking)  
    │  
    ├── ConcurrentSkipListMap<K, V>   (implements NavigableMap, thread-safe, uses a Skip List)  
```
## Hierarchy of Map - Interfaces:
```mathematica
Map<K, V>  
    ▲  
    ├── SortedMap<K, V>   (extends Map, maintains sorted key order)  
    │    ▲  
    │    ├── NavigableMap<K, V>   (extends SortedMap, adds navigation methods like lowerKey(), ceilingKey())  
    │         ▲  
    │         ├── ConcurrentSkipListMap<K, V>   (implements NavigableMap, thread-safe, uses Skip List)  
    │  
    ├── ConcurrentMap<K, V>   (extends Map, supports atomic operations for concurrency)  
         ▲  
         ├── ConcurrentHashMap<K, V>   (implements ConcurrentMap, high-performance thread-safe hash table)  
```
