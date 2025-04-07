
```mathematica
    Map<K, V> // Root Interface
    ▲
    ├── SortedMap<K, V> // Extends Map, maintains sorted key order
    │    ▲
    │    ├── NavigableMap<K, V> // Extends SortedMap, adds navigation methods like lowerKey(), ceilingKey(), etc.
    │         ▲
    │         ├── TreeMap<K, V>   (implements NavigableMap, extends AbstractMap, uses Red-Black Tree for sorting keys)
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

It introdues the following methods (in addtion to methods inherited from Collection interface, it has methods named the same but the ones here use Key, instead of Object, such as remove):
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

### ConcurrentMap

It extends the Map interface and is specifically designed for use in concurrent (multithreaded) environments. It provides additional methods that allow safe and efficient operations in scenarios where multiple threads might be accessing or modifying the map simultaneously.
Therefore ConcurentMap is essentially like a Map, but for multithreading with methods that are usefull when working with multiple threads.

- The ConcurrentMap interface is designed to handle concurrent access from multiple threads, ensuring that operations like put(), get(), and remove() are safe and do not lead to inconsistent states.
- ConcurrentMap provides several **atomic operations** that allow for updates to be made to the map in a thread-safe manner without locking the entire map. These operations typically modify the map based on the presence or absence of keys, and they are designed to handle concurrency efficiently.

Atomic operations are operations that execute as a single, indivisible step, meaning they cannot be interrupted by other threads. In multithreading, atomic operations are crucial because they prevent race conditions—situations where multiple threads try to read/write shared data at the same time, leading to inconsistent results. They are used to perform thread-safe operations without locking the entire map.

How can atomic methods be indivisible and not interrupted, yet still not block other threads? The key lies in how they achieve this.

When we say an atomic operation is indivisible, we mean that once it starts, it completes in a single step at the hardware level. Other threads cannot observe a partial or intermediate state—it’s either entirely done or never happened. However, this does not mean that atomic operations block other threads like a synchronized lock does. Instead, they use low-level CPU instructions (like Compare-And-Swap, or CAS) to make updates without locking the entire data structure.

So atomic methods are operations that fully complete behind the scenes without locking the Map, but at the same time guarantee the complition of the method.

Key Characteristics of Atomic Methods:
1. Indivisibility – The operation either completes fully or doesn’t happen at all. No other thread can see a half-finished update.
2. Thread Safety – Atomic methods ensure that no two threads can interfere with each other while performing an operation.
3. No Partial Updates – Other threads will never see an incomplete modification.
4. No Locks Needed (in some cases) – Atomic operations often use low-level CPU instructions instead of locks, making them faster than using synchronized blocks

All methods in ConcurentMap are inherited from Map.

How does ConcurentMap achieve multithreading compared to Map? While Map would typically require synchronization if accessed by multiple threads, ConcurrentMap uses more efficient, fine-grained locking mechanisms (like segment locking in ConcurrentHashMap) to allow multiple threads to work on different parts of the map simultaneously.

### SortedMap

A specialized version of the Map interface that guarantees the keys in the map are ordered in a specific way. The key difference between a SortedMap and a regular Map is that a SortedMap ensures that its keys are stored in a defined order, either in their natural order (i.e., the natural ordering of the keys as defined by Comparable), or according to a custom Comparator provided at the time of map creation.

Methods for Navigating the Map:
- SortedMap adds several methods that help navigate the sorted keys. These methods are:
- firstKey(): Returns the first (lowest) key in the map.
- lastKey(): Returns the last (highest) key in the map.
- headMap(K toKey): Returns a view of the map containing keys less than the specified key toKey.
- tailMap(K fromKey): Returns a view of the map containing keys greater than or equal to the specified key fromKey.
- subMap(K fromKey, K toKey): Returns a view of the map containing keys between fromKey (inclusive) and toKey (exclusive).

These methods provide more flexibility in accessing subsets of the map compared to a regular Map.

### NavigableMap

It's an extension of SortedMap<K, V> that provides navigation methods to find entries before or after a given key. It enables efficient range queries, floor/ceiling lookups, and reverse order views. It maintains keys in sorted order from SortedMap, and adds methods for finding closest matches and reverse order views.

Methods for fiding nearest entry:
- lowerKey(K key) - Largest key strictly less than key (for example: lowerKey(25) → 20)
- floorKey(K key) - Largest key less than or equal to key (for example: floorKey(30) → 30)
- ceilingKey(K key) - Smallest key greater than or equal to key (for example: ceilingKey(25) → 30)
- higherKey(K key) - Smallest key strictly greater than key (for example: higherKey(30) → 40)

Methods for reversing order:
- descendingMap() - Returns a reversed-order view of the map
- descendingKeySet() - Returns a reversed-order set of keys

Submap Methods:
- subMap(K fromKey, boolean fromInclusive, K toKey, boolean toInclusive) - Gets a range of keys between fromKey and toKey (for example: subMap(20, true, 40, false) → {20, 30})
- headMap(K toKey, boolean inclusive) - Gets entries before toKey (for example: headMap(30, true) → {10, 20, 30})
- tailMap(K fromKey, boolean inclusive) - Gets entries from fromKey onward (for example: tailMap(30, false) → {40, 50})

-------------------------------

## Classes

### HashMap

Is a key-value pair data structure that allows for constant-time (O(1)) lookups, insertions, and deletions in the average case. It achieves this efficiency using a hash table and a hashing function. It implements its own hash table internally rather than using a pre-built hash table from another Java class or interface.

Extends AbstractMap<K, V> → Gets default implementations for many Map methods.
Implements Map<K, V> → Ensures it follows Map contract.
& Implements Cloneable & Serializable → Can be cloned and serialized.
```java
public class HashMap<K, V> extends AbstractMap<K, V>
                           implements Map<K, V>, Cloneable, Serializable
```

Keys are converted into an array index using a hash function. The hash table stores buckets, each bucket holds entries (key-value pairs). Each bucket uses a linked list or binary tree (since Java 8) for collision handling. 

How Does HashMap Implement a Hash Table?
- HashMap defines its own array-based hash table internally.
- It stores buckets in an array of Node<K, V>[] (before Java 8, it was Entry<K, V>[]).
- The array is indexed using a hashing function based on the key’s hashCode().
- When collisions occur, it stores entries in a linked list (or a Red-Black tree if needed in Java 8+).
- The Node<K, V>[] table (an array of these Node objects) represents the hash table. Take a look:
```java
static class Node<K, V> implements Map.Entry<K, V> {
    final int hash;  // Precomputed hash for faster lookups
    final K key;     // The key
    V value;         // The associated value
    Node<K, V> next; // Next node in case of a collision (linked list)
}
```
- Java does not use a separate HashTable class - everything is built into HashMap itself.

🪣 Buckets in HashMap
A HashMap stores key-value pairs in an internal array called table, which consists of buckets.
Each bucket is a location in that array (think: an index).
A hash of the key determines which bucket an entry goes into:
HashMap doesn't have ordering, it's not thread safe, it has quick lookup speed O(1) avg, and has medium memory usage.
```java
final int h = hashCode(key);
final int hash = (h ^ (h >>> 16));
int bucketIndex = hash & (table.length - 1);
```
What happens when multiple keys hash to the same bucket?
This is called a hash collision. Java handles this in stages:
1. If two keys hash to the same bucket, their entries are stored in a singly linked list at that index. New entries are added to the front of the list (like a stack). When searching for a key, Java walks through this list using equals().
2. When a single bucket's linked list becomes too long, it's inefficient to traverse linearly, so Java converts it into a Red-Black Tree for better performance. If the number of entries in a bucket exceeds 8, and the total HashMap capacity is at least 64, the linked list is transformed into a red-black tree. If a tree's size drops below 6 (due to deletions), it gets converted back into a singly linked list.

⚙️ Why Red-Black Tree?
A red-black tree ensures O(log n) performance for get/put/remove in that bucket.
It maintains balance (like a self-balancing binary search tree) to avoid worst-case performance.

🤖 A simplified lifecycle of a bucket:
+ Empty: No entry yet
+ Single Entry: One node
+ Few Entries: Stored in a linked list
+ Many Entries: Treeified into a red-black tree
+ Back to Few: If shrunk, detreeify into linked list

The bucket number in a HashMap is directly tied to the hash code of the key, and it is calculated by applying the modulo operation on the hash code with the size of the internal bucket array (table.length). Hash collisions can occur when two distinct keys produce the same bucket number. In this case, the HashMap stores those entries in a linked list (or red-black tree if there are many collisions). You cannot directly modify the bucket number calculation in a HashMap. However, you can control the resulting bucket index by providing a custom hashCode() implementation for your custom key class.
<br><br>
Even though the keys are unique, collisions can still occur in a HashMap, particularly when you have a large number of entries like a million. This is because the bucket index is derived from the hash code of the key, and there are only a finite number of buckets available in the HashMap (determined by the size of the internal table, often a power of 2). Therefore, different keys may end up being hashed to the same bucket index, resulting in a hash collision.

##### How to prevent hash collision?

##### When to Use HashMap?
+ When you need fast lookups (O(1) average time complexity).
+ When ordering of elements does not matter.
+ When using non-thread-safe environments (use ConcurrentHashMap for threads).
+ When handling large datasets efficiently.

It doesn't introduce any new methods.

### LinkedHashMap

LinkedHashMap<K, V> extends HashMap<K, V> and also inherits from AbstractMap<K, V>. Here's the class hierarchy for LinkedHashMap:
```java
public class LinkedHashMap<K, V> extends HashMap<K, V>
                               implements Map<K, V>
```

- Extends HashMap<K, V>: LinkedHashMap inherits the core functionality of HashMap, which uses a hash table for storing key-value pairs.
- Implements Map<K, V>: It implements the Map interface, providing all the methods required by the Map interface, such as put(), get(), remove(), etc.
- Maintains Insertion Order: The key feature of LinkedHashMap is that it maintains the insertion order of elements. This is achieved by maintaining a doubly-linked list that keeps track of the order of the entries as they are inserted.
- LinkedHashMap is backed by both a hash table (like HashMap) and a linked list (for maintaining insertion order).
- It uses the linked list to maintain the order of entries, while the hash table ensures efficient lookups (O(1) for put(), get(), and remove()).
- It does not extend AbstractMap directly because it needs the more specialized hash table-based operations that are already provided by HashMap. AbstractMap is used more for general-purpose map implementations.

The methods of LinkedHashMap are slightly slower than those of HashMap, but the difference is generally quite small. And maintains O(1) time complexity just as HashMap.

Methods: LinkedHashMap inherits most of its methods directly from Map and HashMap.
- It overrides iterator() and put() to ensure that entries are iterated or inserted in a specific order.
- removeEldestEntry() is a custom method in LinkedHashMap that allows users to define a policy for removing entries, but it is optional. It's a protected method that is not implemented by default, instead implementation is left to the user.

### TreeMap

TreeMap is a class that implements the NavigableMap interface and stores key-value pairs in sorted order according to the natural ordering of the keys, or by a Comparator provided at map creation time.

Things to know:

+ Sorting: Maintains keys in ascending order
+ Underlying Data Structure: Red-Black Tree (self-balancing binary search tree)
+ Allows nulls?:
+ Null keys ❌ not allowed (throws NullPointerException)
+ Null values ✅ allowed

More about Red-Black Tree:
TreeMap is implemented using a Red-Black Tree, which is a type of self-balancing binary search tree. And just like a linked list is made of nodes pointing to the next/previous element, a TreeMap internally maintains a collection of nodes, each representing a key-value pair with left/right/parent references. Each node is an instance of a nested class called TreeMap.Entry<K, V>, and it looks something like this:
```java
static final class Entry<K,V> implements Map.Entry<K,V> {
    K key;
    V value;
    Entry<K,V> left;
    Entry<K,V> right;
    Entry<K,V> parent;
    boolean color; // true = RED, false = BLACK
}
```
When you put() a new entry, the tree uses binary search to find the correct spot based on the key's order (via Comparable or a Comparator). After insertion, the Red-Black Tree rebalances itself using rotations and recoloring, to keep the tree roughly balanced. Lookup (get()), insert (put()), and remove (remove()) all have O(log n) time complexity thanks to this balance.

Red-Black Trees are very efficient. They have fast storage and retrieval of ordered information. They take a number and split the remaining numbers into two groups - numbers greater than this number and lesser than root number. The root is always black. Red numbers are those whose both children are black, and black numbers are those that have no children. Each following number has numbers flowing from it based on the same principle that the root number has, with proximity of the numbers being the determining factor on which number gets to be a leaf (children) of which parent. <br>
![image](https://github.com/user-attachments/assets/2dda5929-9a78-4a17-93eb-2f906e70eac7)
<br>
They work for any type of data, as long as the data can be ordered - which is possible for almost all values since TreeMap uses a Comparator, you just can't mix the values - like using String and Integer.

### ConcurrentHashMap

ConcurrentHashMap is a thread-safe implementation of the ConcurrentMap interface. It's used when multiple threads need to access and modify a map without external synchronization (i.e., no need to wrap it with Collections.synchronizedMap()).

Things to know:
+ It does not allow null keys or values (to avoid ambiguity in concurrent operations)
+ It allows concurrent read and write operations.
+ It does not lock the whole map on every write like Hashtable.
+ It offers atomic operations like putIfAbsent, compute, and merge.
+ It uses a lock-free, non-blocking algorithm with CAS (Compare-And-Swap) and fine-grained locks.
+ Uses a hash table of Nodes or linked lists (or trees when collisions are high).
+ Converts a bucket's list into a balanced tree (like Red-Black Tree) when many keys hash to the same bucket (just like HashMap).

❌ Limitations:
+ Slightly slower than HashMap in single-threaded scenarios (because of thread-safety overhead)
+ You cannot use null as a key or value

### More classes will be added if needed.
