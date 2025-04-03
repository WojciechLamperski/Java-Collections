## List Intreface and its classes:

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

## CopyOnWriteArrayList

It's a thread-safe variant of ArrayList in Java. It is part of the java.util.concurrent package, which provides classes designed for concurrent programming in multi-threaded environments. It is designed for use cases where reads are frequent, and writes (additions, deletions, or updates) are infrequent, making it suitable for scenarios where there is a need for high concurrency and thread safety without using synchronization.

When to Use CopyOnWriteArrayList
- Read-Heavy Workloads: Ideal for use cases where reads vastly outnumber writes. For example, if you have a collection that is frequently accessed by multiple threads but rarely modified, CopyOnWriteArrayList can provide a high-performance solution with built-in thread safety.
- Immutability Requirements: If you need a data structure that provides immutability (e.g., to prevent modification during iteration), CopyOnWriteArrayList is useful because it allows safe iteration without concern for external modifications.

Performance Characteristics
- Read Operations: O(1) – Since the internal array remains unchanged during reads, accessing elements is very fast, and no locks are required for read operations.
- Write Operations: O(n) – Write operations such as add(), remove(), and set() require copying the entire array, which is an O(n) operation. This can become costly if write operations are frequent.
- Memory Overhead: The list may require more memory than a normal ArrayList due to the need to maintain copies of the array during write operations.

Since CopyOnWriteArrayList extends AbstractList and implements List, it inherits most of the methods from List, as well as AbstractList. 
