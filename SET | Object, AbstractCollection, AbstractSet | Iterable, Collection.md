## Set, and all the Classes and Interfaces needed to understand Collection Hierarchy

### Object
All collections are objects because their implementations are classes, and all classes implicitly inherit from Object (which is a class). While collection interfaces (List, Set, etc.) do not inherit Object methods, any concrete implementation we use (ArrayList, HashSet, etc.) does. This ensures that methods like .equals() and .hashCode() are always available in actual collection objects, since they are intherited from Object.

#### hashCode(): 
Returns an integer that represents the object's hash value, which is used for efficient lookup in hash-based collections like HashSet and HashMap. By default, Object.hashCode() in Java returns a unique integer derived from the object's memory address, but classes often override hashCode() to generate a value based on relevant fields (e.g., a string’s characters or a number’s value) to ensure logical equality. This allows different instances with the same data to be treated as duplicates in collections.

Not every object has a unique hashCode(). While Java's default implementation (Object.hashCode()) may return a unique value based on memory address, custom hashCode() implementations can produce the same hash for different objects. This is known as a hash collision.
If hashCode() is based on limited fields (e.g., only a name in a Person class), different objects with the same value will have the same hash.

**Pigeonhole Principle** (Finite Hash Space): hashCode() returns an int (32-bit), meaning there are only 2³² possible hash values. Since Java programs can create more than 2³² objects, collisions are inevitable.

Also default Object.hashCode() Is Not Always Unique. It is based on an internal mechanism (often the memory address but not guaranteed), and in rare cases, objects may have the same hash.

Objects are stored in Heap memory, which is shared across threads and used for dynamic memory allocation.
References (pointers to objects) are stored in Stack memory, which is specific to a thread and contains method call frames and local variables.

#### equals(): 
- Equals for primitives
equals() does not work for primitives because primitives are not objects. Instead, Java provides relational operators (==, >, <, etc.) for primitive comparisons. Hence, why you can't store primitives in Java collections.
- Equals for objects
equals() compares values (content) of objects, whereas == compares memory references. If a class does not override equals(), it inherits Object.equals(), which performs a reference check (==).
```java
class A {}
A obj1 = new A();
A obj2 = new A();
System.out.println(obj1.equals(obj2)); // false (because different memory locations)
System.out.println(obj1 == obj2);      // false (different objects)
```
- Equals for Strings
Java overrides equals() in String to compare contents, not memory addresses.
```java
String s1 = new String("Hello");
String s2 = new String("Hello");
System.out.println(s1.equals(s2)); // true (same content)
System.out.println(s1 == s2);      // false (different memory locations)
```
```java
However, String literals are stored in the String Pool, so when assigned directly:
String s3 = "Hello";
String s4 = "Hello";
System.out.println(s3 == s4);      // true (same reference due to String Pool)
System.out.println(s3.equals(s4)); // true (same content)
```
- Equals for wrapper classes
Wrapper classes (Integer, Double, etc.) override equals() to compare values instead of references.
```java
Integer x = 1000;
Integer y = 1000;
System.out.println(x.equals(y)); // true (compares value)
System.out.println(x == y);      // false (different objects)
```
📝 Note: Small integers (-128 to 127) are cached, so == may return true for values in that range:
```java
Integer a = 127;
Integer b = 127;
System.out.println(a == b); // true (cached values)
```
---------------------------------------------------------

Collections are split between interfaces and classes, but only classes can be used in our code, but we can define a type to be interface for flexibility
```java
List<String> myList;
myList = new ArrayList<>();
myList = new LinkedList<>();
```

I'm not sure if the above is the cleanest / recommended approach, but technically we can do that. What we can't do is this:
```java
List<String> myList = new List<>(); // ❌ Compilation error
```
--------------------------------------------------------- 

## All Collections implement Iterable. Below we'll explore Iterable and Collections Interfaces:

### Iterable
Iterable<E> (The Foundation)
Interface that allows iteration over elements.
Implemented by Collection<E> (which means all collections are iterable).

Provides the methods:

- iterator(): which returns Iterator<E> (not Iterable but ITERATOR), allowing elements to be accessed sequentially using Iterator methods:
// iterator() returns Iterator, just as below:
Iterator<E> iterator = collection.iterator();
// Example of accessing elements sequentially using hasNext:
while (iterator.hasNext()) {
    System.out.println(it.next());
}

These Iterator methods are as follows:
public interface Iterator<E> {
    boolean hasNext(); // Checks if there's another element
    E next();          // Returns the next element
    void remove();     // (Optional) Removes the current element
}

The speecifics of how these methods are implemented depend on the specific collection,
which define them using @Override annotation, but their effect is the same regardless of the class.

- forEach(Consumer<? super E> action): Enables lambda-based iteration. Under the hood it does that:
Iterator<T> iterator = collection.iterator();
while (iterator.hasNext()) {
    action.accept(iterator.next());
}

- spliterator(): Supports parallel processing. This method returns an instance of a Spliterator implementation that is specific to that collection type.

public interface Spliterator<T> {
    boolean tryAdvance(Consumer<? super T> action); // Process one element at a time
    void forEachRemaining(Consumer<? super T> action); // Process remaining elements
    Spliterator<T> trySplit(); // Split into two parts for parallelism
    long estimateSize(); // Estimate number of elements
    int characteristics(); // Returns characteristics (ORDERED, DISTINCT, etc.)
}

Similarly to Iterator each collection provides a custom implementation of Spliterator.
Splitarator is used for parallel streams, where the workload is divided.


### Collection:
The Collection interface extends Iterable, meaning it inherits the ability to use forEach(), iterator(), and spliterator(). However, Collection adds additional functionality that makes it more powerful and useful for working with groups of objects.

+ Bulk Operations: Unlike Iterable, Collection allows modifying groups of elements at once:
- add(E e): Adds a single element
- addAll(Collection<? extends E> c): Adds all elements from another collection
- remove(Object o): Removes a single element
- removeAll(Collection<?> c): Removes all elements found in another collection
- retainAll(Collection<?> c): Keeps only elements found in another collection
- clear(): Removes all elements

+ Query Operations: Collection also provides methods to check contents and size, which Iterable does not:
- contains(Object o): Checks if an element exists
- containsAll(Collection<?> c): Checks if all elements exist
- isEmpty(): Returns true if empty
- size(): Returns number of elements

+ Converting to an Array: Collection also allows converting to an array, which Iterable does not:
- toArray(): Returns an array of elements
- <T> toArray(T[] a): Returns elements in a given array type

+ Stream Support: Although Iterable allows forEach(), Collection introduces streams, which enable functional-style programming.
- stream(): Returns a sequential stream
- parallelStream(): Returns a parallel stream

#### AbstractCollection

AbstractCollection is a class that implements some methods that Collection interface defines, such as:
- isEmpty() - returns size() == 0, 
- contains(Object o) - uses iterator() to check for o, 
- toArray(), 
- toString(),
- remove(Object o) - uses iterator().remove(), 
- clear() - calls iterator.remove() on all elements, 
- addAll(Collection<? extends E> c) - calls add(e) for each element.
A lot of these methods as you can see are dependent on **size()**, **add(e)** & **iterator()** methods which are defined by individual collection subclasses later down the extension chain.
---------------------------------------------------------

## Set

There are various collection interfaces that ensure, different behaviours of collection classes. 
Let's start with the category of classes that implement Set interface:

+ Set: Extends Collection<E>. Ensures unique elements (no duplicates).
- How does Set guarantee uniquness of its values? 
A Set ensures uniqueness by relying on equals() and hashCode(). When you add an element, the Set checks whether it is already present using these methods.
1. Compute hashCode()
If the Set is a HashSet, it uses hashCode() to determine which "bucket" to place the element in.
If two objects have different hash codes, they must be different.
If they have the same hash code (collision), equals() is used.
2. Call equals() for Collision Resolution
If another object is already in the same hash bucket, equals() is called.
If equals() returns true, the new object is not added.
If equals() returns false, the new object is added.

- Does Set Use Shallow or Deep Comparison when deciding which elements to add and which ones to keep?
Shallow compare. It doesn't check the fields only if the two have different hashCode, and if they don't then it checks
equals() compares values (content) of objects, (by the way == compares memory references).
If a class does not override equals(), it inherits Object.equals(), which performs a reference check (==).
However in most cases it does overwrite it, the only ones that don't are Queues (PriorityQueue, Deque, etc.).

For example Sets, Lists, and Maps override the default equals, to do an equals on each of these collections elements, instead of doing it on the lists themeselves. So list1.equals(list2), in simplified terms does something like list1(object1.equals(object2), string1.equals(string2), ...), which, under the hood, is implemented like this: 
for (int i = 0; i < list1.size(); i++) {
    if (!list1.get(i).equals(list2.get(i))) {
        return false;
    }
}
return true;

Java iterates through each element and calls equals() on them. If the elements don't override equals(), the default reference (==) comparison is used. To achieve a true deep comparison (comparing internal fields), we must override equals() in our objects.

- Methods unique to Set: 
* Actually, Set does not introduce any new methods that aren't already inherited from its parent interfaces!
* Instead of adding new methods, Set changes the behavior of existing ones:
* No Duplicates: add(E e) ensures only unique elements. ✅
* Modified equals() and hashCode(): Equality is based on elements, not order. ✅

### AbstractSet

AbstractSet is a class that actually most Set collections use. AbstractSet is an implementation of Set, and extension of AbstractCollection, which is an implementation of Collection.
HashSet, LinkedHashSet, and TreeSet all extend AbstractSet. But there are some collections that don't, such as:
- EnumSet which implements Set directly for better performance with enums.
- ConcurrentSkipListSet which implements NavigableSet directly, optimized for concurrency.
- CopyOnWriteArraySet which implements Set directly for thread safety.
 
What is the difference between extending AbstractSet and implementing Set directly,? 

Extending AbstractSet vs. Implementing Set directly comes down to trade-offs in performance, memory usage, and special behavior. Implementing Set directly is often chosen for performance-critical or concurrent collections.
There are 2 classes that implement Set directly (which we will discuss later), instead of using AbstractSet:
- EnumSet:
   + Uses bitwise operations instead of a general-purpose Set implementation.
   + Extending AbstractSet would add unnecessary method overhead.
   + Implements Set directly to be as fast as possible for enums.
- CopyOnWriteArraySet:
   + Uses a copy-on-write strategy (modifies a new copy of the array for each write).
   + Implements Set directly so it can be backed by CopyOnWriteArrayList without unnecessary methods from AbstractSet.
   + Extending AbstractSet would not help, as AbstractSet is not designed for concurrent modifications.

It inherits all its methods from Set interface.

### SortedSet & NavigableSet 

Sorted Set implements Set.
SortedSet guarantees elements are sorted based on natural ordering or a comparator.
Insertion order ≠ Sorted order
Consider the below comparison of Set implementation (that also has ordering) and SortedSet

Set<Integer> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add(3);
linkedHashSet.add(1);
linkedHashSet.add(2);
System.out.println(linkedHashSet); 
// ✅ Output: [3, 1, 2] (Maintains insertion order, NOT sorted)

SortedSet<Integer> sortedSet = new TreeSet<>();
sortedSet.add(3);
sortedSet.add(1);
sortedSet.add(2);
System.out.println(sortedSet); 
// ✅ Output: [1, 2, 3] (Sorted order)

- No duplicates allowed ❌
- Sorted order (natural or custom)
- Sorted (by Comparable or Comparator) ✅

Custom methods:
- first(), 
- last(), 
- headSet(), 
- tailSet(), 
- subSet()

### NavigableSet

What is NavigableSet?

NavigableSet<E> is an interface that extends SortedSet<E> (which isn't used directly by a single class).
It adds extra navigation methods that allow you to find elements before/after a given value.

Key Methods in NavigableSet (used by TreeSet & ConcurrentSkipListSet)

- lower(E e) - Returns greatest element < e
- floor(E e) - Returns greatest element ≤ e
- ceiling(E e) - Returns smallest element ≥ e
- higher(E e) - Returns smallest element > e
- descendingSet() - Returns a reverse-order view of the set

---------------------------------------------------------

## Set hierarchy in detail:
```mathematica
Iterable<E>  
    ▲  
Collection<E>  
    ▲ 
Set<E>  
    ▲  
    ├── HashSet<E>   (extends AbstractSet, uses HashMap internally)  
    │  
    ├── LinkedHashSet<E>   (extends HashSet, uses LinkedHashMap)  
    │  
    ├── TreeSet<E>   (implements NavigableSet, extends AbstractSet, uses Red-Black Tree)  
    │  
    ├── EnumSet<E>   (implements Set directly, uses bitwise operations)  
    │  
    ├── CopyOnWriteArraySet<E>   (implements Set directly, uses CopyOnWriteArrayList)  
    │  
    ├── ConcurrentSkipListSet<E>   (implements NavigableSet (which implements SortedSet<E>) directly, uses Skip List)  
```
Remember that AbstractSet is an implementation of Set, and an extension of AbstractCollection which is an implementation of Collection
<br />

```mathematica
Set<E>  
    ▲ 
AbstractSet<E> (extends AbstractCollection)
```
Remember that AbstractSet is the class responsible for providing default implementations of hashCode() and equals() for Set collections.
<br />

```mathematica
Collection<E>  
    ▲ 
AbstractCollection
```
----------------------------------------------------------------

## Implementations of Set, and/or NavigableSet and extensions of AbstractSet

### HashSet<E>
#### ⏰ Performance & Time complexity: Provides O(1) average-time complexity for add, remove, and contains operations. Iteration is O(n) and typically slightly faster than LinkedHashSet (no linked-list overhead), but slower than TreeSet for ordered traversals.

- Implements Set<E>, backed by a HashMap (keys are the elements, values are dummy objects).
- Unordered, allows null.
- add(), remove(), contains() work in O(1) average time.
- No ordering guarantees.
- When it comes to implementations, yes HashSet uses Set. But there is one more layer that isn't related to implementations. You see HashSet essentially uses HashMap internally, but doesn't implement it per se. HashSet doesn't directly store elements. Instead, it uses a HashMap where:
* The elements are stored as keys in the HashMap.
* A constant dummy object (PRESENT) is used as the value.
* Why use HashMap? Because HashMap already ensures unique keys using hashCode() and equals().
* Fast operations (O(1)) due to hashing. ✅
* No ordering is preserved. ❌
* HashSet internally does something like this:
```java
private transient HashMap<E, Object> map;
private static final Object PRESENT = new Object(); // Dummy value

public boolean add(E e) {
    return map.put(e, PRESENT) == null;
}
```

HashSet inherits all its methods from AbstractSet class.

### LinkedHashSet<E>
#### ⏰ Performance & Time complexity: Offers O(1) time complexity for add, remove, and contains (like HashSet), but maintains insertion-order iteration with O(1) per-element overhead. Iteration (forEach, iterator) is O(n) but faster than HashSet due to linked-list traversal optimization.

- Extends HashSet<E>, maintains insertion order. It doesn't sort elements.
- Uses a LinkedHashMap internally (doubly linked list to preserve order).
- Slightly slower than HashSet due to ordering overhead.
- LinkedHashSet extends HashSet (which uses HashMap), but Adds a Doubly Linked List. So LinkedHashSet extends HashSet, and overrides (only) HashSet’s internal HashMap usage and replaces it with LinkedHashMap - everything else remains the same.
* Preserves insertion order (because LinkedHashMap does). ✅
* Still O(1) for most operations, but with slightly more memory overhead due to the linked list. ✅
* Internally LinkedHashSet's usage of LinkedHashMap looks like that:
```java
public class LinkedHashSet<E> extends HashSet<E> {
    private transient LinkedHashMap<E, Object> map; // Uses LinkedHashMap instead

    public boolean add(E e) {
        return map.put(e, PRESENT) == null; // Uses LinkedHashMap to maintain order
    }
}
```
It inherits all its methods from AbstractSet class.

### TreeSet<E>
#### ⏰ Performance & Time complexity: Almost all of it's methods have O(log n) time complexity (besides first() and last() methods) It's less performance efficient than HashMap.
 

- Implements SortedSet<E>, backed by a Red-Black Tree.
- Maintains sorted order (natural/comparator).
- add(), remove(), contains() work in O(log n) time.
- TreeSet uses a mechanism called Red-Black Tree to store and retrieve elements:

* A Red-Black Tree is a self-balancing binary search tree (BST) used in TreeSet and TreeMap to maintain sorted order while keeping operations efficient.
 * Key Properties of a Red-Black Tree
 * To maintain balance, a Red-Black Tree follows these rules:
   - Each node is either red or black.
   - The root node is always black.
   - Red nodes cannot have red children (no two red nodes in a row).
   - Every path from root to a null (leaf) node must have the same number of black nodes.
   - On insertions & deletions, rotations and color changes may be performed to maintain balance.
 * Guarantees O(log n) time complexity for insert, delete, and lookup operations. ✅ 
 * Always balanced, preventing the worst-case O(n) lookup time of a regular BST (Binary Search Tree). ✅
 * Automatically sorts elements while keeping performance efficient. ✅ 
 * Similarly to other Set implementations TreeSet uses a Map implementation. Specifically **TreeSet in Java relies on a TreeMap**, which internally implements a Red-Black Tree. This ensures that elements are sorted, balanced, and accessible in O(log n) time.
 * The main thing that differentiates TreeSet from TreeMap is that TreeSet prevents duplicate elements, while TreeMap prevents duplicate keys.
 ```java
 public class TreeSet<E> extends AbstractSet<E> implements NavigableSet<E> {
    private transient NavigableMap<E, Object> map;  // Uses a TreeMap
    private static final Object PRESENT = new Object(); // Dummy value

    public TreeSet() {
        this.map = new TreeMap<>();
    }

    public boolean add(E e) {
        return map.put(e, PRESENT) == null; // Inserts key into TreeMap
    }
}
```

TreeSet has operations like **floor()** - Returns the greatest element ≤ e (or null if none exists) - and **ceiling()** - Returns the smallest element ≥ e (or null if none exists) which other classes don't have. Take a look:

Other methods include:
- E first() → O(1) (smallest element).
- E last() → O(1) (largest element).
- E floor(E e) → Greatest element ≤ e (O(log n)).
- E ceiling(E e) → Smallest element ≥ e (O(log n)).
- E lower(E e) → Greatest element < e (O(log n)).
- E higher(E e) → Smallest element > e (O(log n)).

Subset/Range Views (O(log n + k))
- SortedSet<E> headSet(E toElement) → Elements < toElement.
- SortedSet<E> tailSet(E fromElement) → Elements ≥ fromElement.
- NavigableSet<E> subSet(E fromElement, E toElement) → Range [from, to).

Iteration & Bulk Operations
- Iterator<E> iterator() → Ascending-order iteration (O(1) per element).
- Iterator<E> descendingIterator() → Reverse-order iteration (O(1) per element).
- NavigableSet<E> descendingSet() → Reverse-order view (O(1)).

### ConcurrentSkipListSet
#### ⏰ Performance & Time complexity: O(log n) methods it's relatively slower than HashSet or ConcurrentHashMap since it maintains a sorted order. However, i's fast for to other sorted sets like TreeSet it's much faster for multiple threads. Pick it if you need a sorted, thread-safe set.

Another method that uses NavigableSet is ConcurrentSkipListSet. Instead of using Red-Black Tree it uses a Skip List, a probabilistic data structure designed for fast search, insertion, and deletion. Like TreeSet, elements in ConcurrentSkipListSet are stored in sorted order, using natural ordering or a Comparator. Another thing to consider is Time Complexity: Operations like add(), remove(), and contains() typically have O(log n) time complexity, but due to the probabilistic nature of Skip Lists, the actual performance can vary. However, it's typically considered to be more efficient for concurrent access.

TreeSet is not thread-safe. If multiple threads try to modify a TreeSet concurrently, you'll need to manually synchronize access using synchronized blocks or other concurrency mechanisms (e.g., ReentrantLock).
 ConcurrentSkipListSet is thread-safe by design. It allows multiple threads to read from and write to the set concurrently without the need for external synchronization. The Skip List supports non-blocking operations, which is crucial in concurrent environments where multiple threads may access or modify the set simultaneously.
  
TreeSet is typically used when you need a sorted set and don't require concurrent access by multiple threads. It's suitable for single-threaded applications or scenarios where synchronization is manually handled.  Since TreeSet is not optimized for concurrency, if you need to perform thread-safe operations on it, you may need to manually synchronize blocks of code, which can affect performance in multi-threaded applications.

ConcurrentSkipListSet is designed for scenarios where thread-safe operations are required. It's ideal for multi-threaded applications where multiple threads will be concurrently inserting, deleting, or accessing elements. ConcurrentSkipListSet provides good performance in multi-threaded environments due to its lock-free and non-blocking operations, making it a better choice for high-concurrency use cases compared to TreeSet.

Both TreeSet and ConcurrentSkipListSet implement the NavigableSet interface, so they share common methods like lower(), floor(), ceiling(), higher(), descendingSet(), and others. However, since ConcurrentSkipListSet is thread-safe, it can be used in concurrent environments without requiring external synchronization, while TreeSet cannot be used safely in such environments without additional synchronization.

TreeSet: The internal structure (Red-Black Tree) locks the entire tree or parts of it when performing an operation, which can create contention when multiple threads try to modify the set concurrently. ConcurrentSkipListSet: The internal structure (Skip List) allows concurrent operations by using fine-grained locks or lock-free mechanisms at different levels of the Skip List. This allows multiple threads to access and modify different parts of the list without waiting for a global lock, resulting in better concurrency performance.

TreeSet is ideal when you need a sorted set and don't need thread-safety or plan to manage concurrency externally.
ConcurrentSkipListSet is the preferred choice for multi-threaded applications that need a sorted set and require thread-safe access without the need for external synchronization.

The performance and time complexity of ConcurrentSkipListSet in Java is O(log n) for basic operations like add(), remove(), and contains(), as it is implemented using a concurrent skip list. It provides efficient concurrent access with fine-grained locking, making it well-suited for multi-threaded environments.

#### Comparator (a seperate interface).

Comparator is a seperate Interface and can be used to write a tottaly custom comparator that doesn't depend on TreeSet and ConcurentSkipListSet

Example of using custom comparator in TreeSet:
```java
public class TreeSetCustomComparatorExample {
    public static void main(String[] args) {
        // Create a TreeSet with a custom Comparator
        Set<Person> people = new TreeSet<>(new PersonComparator());

        people.add(new Person(3, "Alice"));
        people.add(new Person(1, "Bob"));
        people.add(new Person(2, "Charlie"));

        // Prints in descending order based on ID (3, 2, 1)
        System.out.println(people);
    }
}
```
Example of using custom comparator in ConcurentSkipListSet:
```java
import java.util.concurrent.ConcurrentSkipListSet;

public class SkipListSetCustomComparatorExample {
    public static void main(String[] args) {
        // Create a ConcurrentSkipListSet with a custom Comparator
        Set<Person> people = new ConcurrentSkipListSet<>(new PersonComparator());

        people.add(new Person(3, "Alice"));
        people.add(new Person(1, "Bob"));
        people.add(new Person(2, "Charlie"));

        // Prints in descending order based on ID (3, 2, 1)
        System.out.println(people);
    }
}
```
### EnumSet<E>  
#### ⏰ Performance & Time complexity: Super fast O(1) operations (due to bitwise operations)
It's a specialized Set implementation exclusively for enum types. It is highly optimized, using bitwise operations (operations on each Enums bit representation) instead of hash-based storage like HashSet.

Key characteristics:
- Backed by a single long (for ≤ 64 elements) or a long[] (for > 64 elements) - so one long element that represents all the bits
- Uses bitwise operations for O(1) performance in add(), remove(), contains()
- No null elements allowed
- Maintains natural enum order
- Instead of a HashMap, it stores elements in a bit vector (A bit vector (also called a bit array, bitset, or bitmap) is a data structure that efficiently stores bits (0s and 1s) in a contiguous block of memory. Each bit in a bit vector represents a binary value, and these bits are typically used to represent a series of boolean flags or other values that can either be true or false.).
- Each enum is assigned a bit position (based on its ordinal value).
- Example. Let's say we have the following enum:
```java
enum Color { RED, GREEN, BLUE, YELLOW } // ordinals: 0,1,2,3
```
Each enum constant gets an ordinal index automatically assigned (that's not part of EnumSet yet):
```
RED    -> ordinal = 0
GREEN  -> ordinal = 1
BLUE   -> ordinal = 2
YELLOW -> ordinal = 3
```
- When you create an EnumSet containing { RED, BLUE }, it is stored as a bit vector in a single long variable.
```
RED    = 0b0001  (1 in decimal)   → Bit at position 0 is set
GREEN  = 0b0010  (2 in decimal)   → Bit at position 1 is set
BLUE   = 0b0100  (4 in decimal)   → Bit at position 2 is set
YELLOW = 0b1000  (8 in decimal)   → Bit at position 3 is set
```
- you get the following methods that use bit-related operatons (no point to dwell on them outside of them being very efficient and O(1) operations):
+ add(E e) - Uses **bitwise OR (`
+ remove(E e) - Uses bitwise AND (&) with NOT (~) to clear the bit
+ contains(E e) - Uses bitwise AND (&) to check if a bit is set
+ size() - Counts the number of set bits
+ iterator() - Iterates over set bits (ordinal values)

- Performance:
+ Super fast O(1) operations (add(), remove(), contains()) due to bitwise operations.
+ Very memory-efficient (especially compared to HashSet).

EnumSet is a specialized implementation of the Set interface designed specifically for storing enum constants (values from an enum type) in Java. It offers very efficient operations compared to other Set implementations due to its internal use of bitwise operations.

### CopyOnWriteArraySet
#### ⏰ Performance & Time complexity: Very fast O(1) & O(n) Read operations - because they don't block other Threads. Costly Write operations O(n) - that's because can't write just a single value but a whole array each time. 

CopyOnWriteArraySet is a thread-safe implementation of the Set interface that uses a copy-on-write strategy to handle modifications. It is optimized for scenarios where reads significantly outnumber writes.

- Backed by CopyOnWriteArrayList<E>, meaning every modification (add, remove, etc.) creates a new copy of the underlying array.
- Thread-safe without explicit synchronization—multiple threads can safely read the set without locking.
- Best suited for scenarios with frequent reads and infrequent writes (e.g., caching, event listener management).
- Iteration provides a snapshot view, meaning iterators never throw ConcurrentModificationException.
- Maintains insertion order, like LinkedHashSet, because it's backed by an array.
- Does not allow duplicates, just like all Set implementations.

CopyOnWriteArraySet<E> is essentially a wrapper around CopyOnWriteArrayList<E>, which manages the elements as follows:
1. On every modification (add, remove, clear, etc.), a new copy of the array is created instead of modifying the existing one.
2. Reads (contains, iteration, size, etc.) are extremely fast because they operate on an immutable snapshot.
3. No locking required for iteration, unlike Collections.synchronizedSet() which requires explicit synchronization.

Best Use Cases:
+ Event listeners: Since they are frequently read but rarely modified.
+ Caching: Read-heavy scenarios where updates are infrequent.
+ Multithreading: Safe to use across multiple threads without locks.
+ Scenarios requiring safe iteration: Since iterators never fail due to concurrent modifications.

Methods:
- add(E e) - Checks for duplicates (contains()). If absent, creates a new copy of the array with e added.
- remove(E e) - Creates a new array copy without e.
- contains(E e) - Performs a linear search (O(n)) on the array.
- size() - Returns the number of elements (O(1)).
- iterator() - Returns an immutable snapshot-based iterator that never fails with ConcurrentModificationException.

Performance Characteristics:
+ Read operations (contains(), size(), iteration) are very fast - O(1) - since they operate on a stable array copy.
 + Read operations don’t require locking or blocking other threads.
 + But they can still be O(n) if scanning the array is necessary (like in contains()).
 + Only direct lookups (like size()) are O(1).  
+ Write operations (add(), remove()) are costly - O(n) - due to array copying.
