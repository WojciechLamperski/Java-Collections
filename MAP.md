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

## Interfaces & Abstract Classes

### Map <K, V>

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

### AbstractMap 

Methods: 
- put(K key, V value): This method is implemented in AbstractMap and simply calls entrySet() to add the key-value pair to the map via an Entry. Subclasses need to provide their own entrySet() implementation to define how the key-value pair is added.
- get(Object key): This method is implemented by calling the entrySet() method and checking each entry. It uses the equals() method to find a matching key and return its corresponding value. Subclasses need to implement entrySet() to define how to access entries.
- containsKey(Object key): This method is implemented in AbstractMap by iterating over the entrySet() and checking for a matching key.
- containsValue(Object value): This method is also implemented by iterating over the entrySet() and checking for matching values.
- remove(Object key): This method is implemented in AbstractMap by iterating over the entrySet() to find and remove the entry with the specified key.
- clear(): This method is implemented in AbstractMap and removes all entries from the map by clearing the entrySet().
- keySet(): This method is implemented in AbstractMap by calling entrySet() and returning a set of the map's keys.
- values(): This method is implemented by calling entrySet() and returning a collection of the map's values.
- entrySet(): This method is abstract in AbstractMap. It must be implemented by subclasses to return a Set of the map's key-value entries.
- putAll(Map<? extends K, ? extends V> m): This method is implemented in AbstractMap by calling entrySet() on the provided map and using put() to add the entries to the current map. It's a default implementation and can be overridden by subclasses if necessary.
- isEmpty(): This method is implemented in AbstractMap by calling the size() method. It's a default implementation that can be used by subclasses.
- size(): The size method is implemented in AbstractMap by calling entrySet(). It returns the number of entries in the map by using entrySet().size().
- toString(): AbstractMap provides a toString() method that converts the map to a string representation. It calls entrySet() to obtain the entries and builds a string representation in the form of {key1=value1, key2=value2, ...}.

Methods not implemented in AbstractMap:
- equals(Object o) and hashCode(): AbstractMap does not provide a default implementation of equals() and hashCode(). These methods are left to subclasses to define based on the specific Map implementation. In most cases, subclasses override these methods to compare Map entries properly.

AbstractMap does not implement all methods from Map. Instead, it provides default (or partial) implementations for several methods, including put(), get(), containsKey(), and others, but requires subclasses to implement the entrySet() method, which is central to how the map works.

-------------------------------

## SortedMap
