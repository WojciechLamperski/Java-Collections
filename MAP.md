## Hierarchy of Map
```mathematica
    Map<K, V> // Root Interface
    ▲
    ├── SortedMap<K, V> // Extends Map, maintains sorted key order
    │    ▲
    │    ├── NavigableMap<K, V> // Extends SortedMap, adds navigation methods like lowerKey(), ceilingKey(), etc.
    │         ▲
    │         ├── TreeMap<K, V>   (implements NavigableMap, extends AbstractSortedMap, uses Red-Black Tree for sorting keys)
    │         ├── ConcurrentSkipListMap<K, V>   (implements NavigableMap, thread-safe, uses Skip List)    
    │
    ├── ConcurrentMap<K, V> // Extends Map, supports atomic operations for concurrency
    │    ▲
    │    ├── ConcurrentHashMap<K, V>   (implements ConcurrentMap, thread-safe, uses segment-based locking)
    │
    ├── EnumMap<K extends Enum<K>, V>   (implements Map directly, optimized for enums, uses an array)
    ├── HashMap<K, V>   (extends AbstractMap, uses hash table + linked list/binary tree for collision handling)
    ├── LinkedHashMap<K, V>   (extends HashMap, maintains insertion order with a doubly linked list)
    ├── WeakHashMap<K, V>   (extends AbstractMap, uses weak references for garbage collection)
    ├── IdentityHashMap<K, V>   (extends AbstractMap, uses reference equality `==` instead of `.equals()`)
    ├── Hashtable<K, V>   (implements Map directly, synchronized, legacy class)

```
-------------------------------

## Map <K, V>

Map<K, V> in Java is conceptually similar to JavaScript’s Object with more built-in methods to make working with key-value pairs easier and more efficient.

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
- clear() - Removes all entries from the map
- size() - Returns the number of key-value pairs in the map
- isEmpty() - Checks if the map is empty
- forEach(BiConsumer<? super K,? super V> action) - Performs the given action for each entry in the map
- putAll(Map<? extends K,? extends V> m) - Copies all the entries from another map into this map
- replaceAll(BiFunction<? super K,? super V,? extends V> function) - Replaces all values in the map using a given function
- computeIfAbsent(K key, Function<? super K,? extends V> mappingFunction) - Computes the value for a key if it is absent
- computeIfPresent(K key, BiFunction<? super K,? super V,? extends V> remappingFunction) - Computes a new value for a key if it's present
- compute(K key, BiFunction<? super K,? super V,? extends V> remappingFunction) - Computes a new value for a key regardless of its current presence


