## List Intreface and its classes:
```java
List<E> // Root Interface  
▲  
├── AbstractList<E>   // (extends AbstractCollection, provides skeletal implementation of List)  
│    ▲  
│    ├── ArrayList<E>   (implements List, extends AbstractList, uses dynamic array)  
│    ├── CopyOnWriteArrayList<E>   (implements List, extends AbstractList, thread-safe, uses copy-on-write strategy)  
│    ├── Vector<E>   (implements List, extends AbstractList, synchronized, legacy class - pick ArrayList instead)  
│         ▲  
│         ├── Stack<E>   (extends Vector, LIFO stack implementation, also a legacy class - use Deque (like ArrayDeque or ConcurrentLinkedDeque))  
│  
├── AbstractSequentialList<E>   // (extends AbstractList, for list implementations with sequential access)  
│    ▲  
│    ├── LinkedList<E>   (implements List & Deque, extends AbstractSequentialList, uses doubly linked list)  
```

### List

The List interface in Java is part of the java.util package and extends the Collection interface. It represents an ordered collection (also called a sequence) of elements, allowing duplicate elements and providing the ability to insert, remove, and access elements by their index position.

Key Characteristics of a List:
+ Order of Elements: A List maintains the order in which elements are inserted. This means that elements in a List are stored in a particular sequence, and the order is preserved when you **iterate** over it.
+ Index-based Access: You can access elements in a List using an index, which is an integer value representing the position of the element in the list. The first element is at index 0, the second element is at index 1, and so on.
+ Duplicates: Unlike sets (which do not allow duplicates), a List can contain duplicate elements. This means you can add the same element multiple times.
Allows null Values: A List allows null values to be stored, except in certain implementations (e.g., CopyOnWriteArrayList can allow null, while some concurrent implementations may not).
+ Most methods in List are inherited from Collection interface, but it does introduce its own methods:

Methods:
- get(int index): Returns the element at the specified position in the list.
- set(int index, E element): Replaces the element at the specified position in the list with the specified element.
- add(int index, E element): Inserts the specified element at the specified position in the list.
- remove(int index): Removes the element at the specified position in the list.
- indexOf(Object o): Returns the index of the first occurrence of the specified element, or -1 if the list does not contain the element.
- lastIndexOf(Object o): Returns the index of the last occurrence of the specified element, or -1 if the list does not contain the element.
- subList(int fromIndex, int toIndex): Returns a view of the portion of the list between the specified fromIndex and toIndex.
- listIterator(): Returns a list iterator over the elements in the list in proper sequence.
- listIterator(int index): Returns a list iterator starting at the specified index.

Why Use List in Java?
- Preserved Order: If you need to store elements in a sequence and want to maintain the insertion order, List is the appropriate choice.
- Efficient Random Access: If you need to frequently access elements by their index position, List allows fast random access.
- Allow Duplicates: If you need a collection that allows duplicate values, List is useful.

### AbstractList
It's an abstract class in the Java Collections Framework that provides a skeletal implementation of the List interface. It is part of the java.util package and serves as a base class for many concrete implementations of the List interface.

While AbstractList provides default implementations for many methods, it relies on subclasses to implement certain essential methods, such as get(int index) and size(), and these are actually the only methods not implemented by AbstractList from the List interface, the implementations of other methods rely heavily on get(int index) and size() leaving the specifics of them to how these two methods will be implemented in individual subclasses. Which is the same as is the case with many other Abstract Classes such as AbstractSet for example.

### ArrayList
ArrayList<E> is a resizable array implementation of the List interface. It provides fast random access, dynamic resizing, and order preservation, making it one of the most commonly used data structures in Java. Essentially a normal array neeeds to know the size of the list beforehand, but ArrayList doesn't require that. It also differs in how it exposes its elements to a user when compared with normal array.

Key Characteristics:
- Implements List<E>, extends AbstractList<E>
- Backed by an array, dynamically resizable
- Allows duplicate elements
- Maintains insertion order
- Allows null values
- Not synchronized (use Collections.synchronizedList() if needed)

Performance (Big-O Complexity):
- Access (get(index)) → O(1) (Direct index-based access)
- Insert at end (add(E e)) → O(1) (Amortized, occasional resizing)
- Insert at specific index (add(index, E e)) → O(n) (Shifting elements)
- Remove by index (remove(index)) → O(n) (Shifting elements)
- Remove by value (remove(Object o)) → O(n) (Search + shift)
- Search (contains(E e), indexOf(E e)) → O(n)

How It Works Internally:
- Uses an array (Object[] elementData) to store elements
- When capacity is exceeded, it grows by 50% of the current size
- When removing elements, it shifts remaining elements to maintain order
- Implements fail-fast behavior (modifications during iteration cause ConcurrentModificationException)

Constructors:
- ArrayList() → Default size 10
- ArrayList(int initialCapacity) → Custom initial capacity
- ArrayList(Collection<? extends E> c) → Initializes with another collection

When to Use ArrayList:
- When you need fast lookups (O(1)),
- dynamic resizing,
- preserve order.

##### 🚫 Not ideal for frequent insertions/removals in the middle → Use LinkedList instead for better performance

##### What are arrays?
In Java, arrays are a fundamental part of the language and can be a bit confusing because they are technically objects, but they have special behavior compared to regular classes. Even though arrays are considered a specific type (like int[], String[], etc.), they are actually objects in Java. All arrays are instances of a class that is a direct subclass of java.lang.Object. Arrays have some special behavior, but underneath the surface, they are just objects. In Java, every array is an instance of a class generated by the JVM. For example, an int[] array is an instance of the class int[], and it can be treated as an object that inherits from java.lang.Object. So Object[]. Arrays also have methods, but these methods are not inherited from Object. Instead, the array class provides methods like length (the size of the array), but you cannot override them because they’re part of the array's implementation in the JVM.


### CopyOnWriteArrayList

It's a thread-safe variant of ArrayList in Java. It is part of the java.util.concurrent package, which provides classes designed for concurrent programming in multi-threaded environments. It is designed for use cases where reads are frequent, and writes (additions, deletions, or updates) are infrequent, making it suitable for scenarios where there is a need for high concurrency and thread safety without using synchronization.

When to Use CopyOnWriteArrayList
- Read-Heavy Workloads: Ideal for use cases where reads vastly outnumber writes. For example, if you have a collection that is frequently accessed by multiple threads but rarely modified, CopyOnWriteArrayList can provide a high-performance solution with built-in thread safety.
- Immutability Requirements: If you need a data structure that provides immutability (e.g., to prevent modification during iteration), CopyOnWriteArrayList is useful because it allows safe iteration without concern for external modifications.

Performance Characteristics
- Read Operations: O(1) – Since the internal array remains unchanged during reads, accessing elements is very fast, and no locks are required for read operations.
- Write Operations: O(n) – Write operations such as add(), remove(), and set() require copying the entire array, which is an O(n) operation. This can become costly if write operations are frequent.
- Memory Overhead: The list may require more memory than a normal ArrayList due to the need to maintain copies of the array during write operations.

Since CopyOnWriteArrayList extends AbstractList and implements List, it inherits most of the methods from List, as well as AbstractList. 
