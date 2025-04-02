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
    │  
    ├── ConcurrentMap<K, V>   (extends Map, supports atomic operations for concurrency)   
```
-------------------------------

## Map <K, V>

Map<K, V> in Java is conceptually similar to JavaScript’s Object, but with fewer restrictions and more built-in methods to make working with key-value pairs easier and more efficient.

K stands for Key. V stands for Value. Key and value can be of any Type, which is more flexible than Object with fields and field values.

It has the following methods:
- put(K key, V value) - Inserts a key-value pair
- get(K key) - Retrieves a value by key
- remove(K key) - Removes an entry by key
- containsKey(K key) - Checks if a key exists
- containsValue(V value) - Checks if a value exists (O(n) for HashMap)
- entrySet() - Returns a set of key-value pairs
- keySet() - Returns a set of keys
- values() - Returns a collection of values
- putIfAbsent(K key, V value) - Adds a new key-value pair only if key is absent
- replace(K key, V value) - Updates an existing key’s value
- merge(K key, V value, BiFunction<V,V,V> remappingFunction) - Merges a value into an existing key
