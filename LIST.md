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

The List interface in Java is part of the java.util package and extends the Collection interface. It represents an ordered collection (also called a sequence) of elements, allowing duplicate elements and providing the ability to insert, remove, and access elements by their index position. Unlike arrays they have flexible size, and don't require to specify a size of List each time you create one.

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

### AbstractSequentialList
It provides a skeletal implementation of the List interface, specifically for lists that are not backed by arrays but by other data structures (e.g., linked lists). It is part of the Java Collections Framework and is used as a base class for lists that store their elements in a sequential manner, such as doubly linked lists or other sequential data structures.

So it's like an AbstractList, but for Classes not backed by Array - ergo not for ArrayList. It assumes that the list elements are stored in a different kind of data structure (like a linked list or a queue).

It focuses on lists (like inked lists or queues) where elements are not stored in contiguous memory locations, such as linked lists, where index-based access is slower but adding/removing elements is more efficient at the start or middle of the list. Its key methods like get(int index) and listIterator(int index) are designed for efficient traversal of sequential data structures.

##### Sequentiality
The concept of sequentiality in AbstractSequentialList refers to how the list is structured and accessed in a linear fashion, typically from one element to the next, in contrast to a structure that allows for random access (like an array or ArrayList). Sequentially-accessed lists (like LinkedList) are typically slower for operations like random access (i.e., accessing an element at a specific index) since you need to traverse the list element by element. However, operations like insertions and deletions in the middle of the list are faster in sequential structures because they don't require shifting elements (as in an ArrayList). Instead, only the links between elements need to be updated.

### ArrayList
ArrayList<E> is a resizable array implementation of the List interface. It provides fast random access, dynamic resizing (like all Lists btw.), and order preservation, making it one of the most commonly used data structures in Java.

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

### LinkedList

It's a class that implements the List interface and extends AbstractSequentialList. It is a doubly linked list implementation, meaning each element in the list points to both the next and the previous element. This enables efficient insertions and deletions from both ends of the list but does not support fast random access like an ArrayList.

It implements List and Deque: LinkedList implements both the List and Deque interfaces. This means that LinkedList can be used as a List (a collection that allows duplicate elements and maintains insertion order) and as a Deque (a double-ended queue that allows insertion and removal of elements from both ends).
- Methods from Deque, such as addFirst(), addLast(), removeFirst(), and removeLast(), allow LinkedList to function as a queue or stack, which adds flexibility to its usage.

No Random Access: Unlike an ArrayList, which allows constant-time access to elements by index, LinkedList provides sequential access. This means that to access an element at a specific index, the list has to be traversed from the beginning (or from the end, whichever is closer) to that index, which is an O(n) operation.

Efficient Insertions and Deletions: LinkedList excels at insertions and deletions, especially for operations at the beginning or end of the list. Since elements in a linked list are stored as nodes that contain references to their neighbors, adding or removing an element from the beginning or end of the list only requires updating a few references, and this happens in O(1) time.

However, inserting or deleting elements from the middle of the list still requires finding the position first (which takes O(n) time) before performing the insertion or removal.

Memory Overhead: Unlike ArrayList, which stores elements in a contiguous array, LinkedList requires extra memory for each element to store the next and previous references (pointers), which can increase memory consumption, especially for smaller lists.

Iterator Support: LinkedList provides an efficient ListIterator, which can traverse the list in both directions (forward and backward). This is especially useful when you need to iterate over the list multiple times, modify it during iteration, or perform operations that depend on the order of elements.

When to use LinkedList:
- Frequent insertions or deletions: If you need to frequently add or remove elements from the beginning or middle of a list, LinkedList can be much more efficient than ArrayList, which requires shifting elements.
- Queue/Deque operations: Since LinkedList implements Deque, it's useful for implementing queues, stacks, or double-ended queues (deques), where elements are added or removed from both ends.
- Ordered traversal: If you need to traverse the list from the beginning to the end (or vice versa), LinkedList offers efficient iteration.

When to avoid LinkedList:
- Random access: If you need fast access to elements at specific indices, LinkedList is not ideal because accessing an element takes O(n) time.
- Memory overhead: Each element in LinkedList requires extra memory for storing references to the next and previous elements.

##### What is a Deque?
A Deque (pronounced "deck") stands for "Double-Ended Queue." It is a queue where you can insert and remove elements from both the front (head) and the rear (tail) of the queue. This is different from a regular queue (FIFO), where elements can only be added at the end and removed from the front.

In a Deque:
- Insertions can happen at both ends: addFirst() for the front and addLast() for the rear.
- Removals can happen at both ends: removeFirst() for the front and removeLast() for the rear.
- Peek operations: You can also peek at the first or last element without removing it, with peekFirst() and peekLast().

LinkedList implements all these methods of Deque, and because it is a doubly linked list, it can efficiently perform these operations at both ends.

The main benefit of a Deque is that it provides efficient access to both ends of the collection. LinkedList, being a doubly linked list, allows both the front and the rear to be modified efficiently in constant time, O(1).

If you need to add or remove elements from only one end of the collection, then a regular Queue or Stack might be better. However, if you need operations at both ends, LinkedList is a great choice because of its flexibility and efficiency.

