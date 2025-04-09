```mathematica
Queue<E> // Root Interface  
▲  
├── Deque<E> // Extends Queue, allows insertion and removal at both ends  
│    ▲  
│    ├── ArrayDeque<E>   (implements Deque, resizable array, faster than Stack & LinkedList for deque operations)  
│    ├── LinkedList<E>   (implements List & Deque, extends AbstractSequentialList, uses doubly linked list)  
│    ├── ConcurrentLinkedDeque<E>   (implements Deque, thread-safe, non-blocking, uses linked nodes)  
│  
├── BlockingQueue<E> // Extends Queue, supports blocking operations for producers/consumers  
│    ▲  
│    ├── LinkedBlockingQueue<E>   (implements BlockingQueue, optionally bounded, uses linked nodes)  
│    ├── ArrayBlockingQueue<E>   (implements BlockingQueue, bounded, backed by an array)  
│    ├── PriorityBlockingQueue<E>   (implements BlockingQueue, unbounded, uses heap for ordering)  
│    ├── DelayQueue<E>   (implements BlockingQueue, unbounded, holds elements until delay expires)  
│    ├── SynchronousQueue<E>   (implements BlockingQueue, no internal capacity, each insert waits for remove)  
│    ├── LinkedTransferQueue<E>   (implements BlockingQueue & TransferQueue, highly concurrent, linked nodes)  
│  
├── PriorityQueue<E>   (implements Queue, extends AbstractQueue, unbounded, uses heap to order elements)  
├── ConcurrentLinkedQueue<E>   (implements Queue, extends AbstractQueue non-blocking, uses linked nodes for thread safety)  
```
-------------------------------

## Interfaces and Abstract Classes

### Queue
The `Queue<E>` interface in Java is part of the java.util package and extends the Collection<E> interface. It represents a FIFO (First-In-First-Out) data structure, where elements are added at the rear and removed from the front.
It is commonly used for task scheduling, buffering, and breadth-first search (BFS) algorithms.

Key Features of Queue
- FIFO Order: Elements are processed in the order they are added.
- Limited vs. Unbounded: Some queues have a fixed size (e.g., ArrayBlockingQueue), while others grow dynamically (e.g., LinkedList or PriorityQueue).
- Blocking and Non-Blocking Queues: Blocking queues like LinkedBlockingQueue wait when the queue is empty or full.
- Non-blocking queues like LinkedList return special values (null or false) when an operation fails.
- Priority-Based Processing: PriorityQueue orders elements based on natural ordering or a comparator.
- Thread-Safety: Some implementations, like ConcurrentLinkedQueue, are designed for multi-threaded environments.

Anti-Use Cases
- ❌ Random Access – Lists (ArrayList, LinkedList) are better.
- ❌ LIFO Behavior – Use Deque (e.g., ArrayDeque, Stack) instead.
- ❌ Fast Lookup by Value – Set or Map would be better choices.

Use Cases of Queue
- ✔ Task Scheduling – Thread pools, event processing.
- ✔ Producer-Consumer Problems – BlockingQueue helps in managing concurrent data.
- ✔ Breadth-First Search (BFS) – Queues help traverse trees and graphs.
- ✔ Message Queues – Background processing and messaging systems (e.g., Kafka).

Outside of what is inheritede from Itereable and Collection Interfaces, Queue introduces the following methods:
- offer(E e): Adds an element, returning false if the queue is full (non-blocking).
- poll(): Retrieves and removes the head of the queue, or returns null if empty.
- peek(): Retrieves, but does not remove, the head of the queue, or returns null if empty.
- remove(): Retrieves and removes the head of the queue, throwing an exception if empty.
- element(): Retrieves, but does not remove, the head of the queue, throwing an exception if empty.

##### Queue Implementations:

- **`LinkedList<E>`** - Implements Queue<E> and Deque<E>, doubly linked list-based. Allows null elements. Suitable for simple FIFO behavior.
  + Uses more memory due to extra node pointers, better for frequent insertions/removals.

- **`PriorityQueue<E>`** - Not strictly FIFO; elements are ordered based on natural ordering or a Comparator. Good for task scheduling where priority is needed.
  + Best for priority-based tasks, but slower than FIFO queues for simple insert/removal.

- **`ArrayDeque<E>`** - Faster than LinkedList for queue operations. Used for both FIFO (Queue) and LIFO (Stack) operations. Does not allow null.
  + Faster than LinkedList, avoids extra memory for node pointers, but resizing can be costly.

- **`ConcurrentLinkedQueue<E>`** - Thread-safe, lock-free non-blocking queue. Uses CAS (Compare-And-Swap) operations for performance in multi-threaded environments.
  + Lock-free, ideal for high-concurrency environments. Slight overhead due to atomic operations.

- **`BlockingQueue<E>`** (Sub-interface of Queue) - Used in multi-threading, blocking when empty or full.
  + Implementations:
     * LinkedBlockingQueue (linked list-based, optional capacity)
     * ArrayBlockingQueue (array-based, fixed capacity)
     * PriorityBlockingQueue (like PriorityQueue but thread-safe)
     * DelayQueue (elements expire after a delay)
   + Best for multi-threaded producer-consumer models. However, blocking operations can introduce thread contention.

### AbstractQueue

AbstractQueue<E> is an abstract class that provides a skeletal implementation of the Queue<E> interface. It serves as a foundation for implementing specific types of queues, offering default implementations for many methods in the Queue interface, but leaving others to be implemented by subclasses. This allows subclasses to focus on the unique parts of queue functionality while relying on the default behavior provided by AbstractQueue for most common queue operations.

Methods Implemented from Queue
+ add(E e): In AbstractQueue, this is implemented by calling the offer(E e) method. The offer method is usually a more efficient way of adding elements to the queue (e.g., it can return a boolean instead of throwing an exception if the operation fails). Default Implementation: return offer(e);
+ offer(E e): This method is typically overridden in concrete implementations to define how elements are added to the queue. If the queue can accept the element, it should return true. If the queue is full, it should return false. Default Implementation: This method is abstract in AbstractQueue, and concrete classes must implement it themselves.
+ remove(): This method removes and returns the head of the queue. If the queue is empty, it throws a NoSuchElementException. Default Implementation: return poll(); (uses poll() which is a safer version that returns null instead of throwing an exception).
+ poll(): This method is implemented in AbstractQueue to retrieve and remove the head of the queue, but it allows returning null if the queue is empty (instead of throwing an exception). Default Implementation: It is abstract in AbstractQueue, so concrete classes must implement it.
+ peek(): This method retrieves, but does not remove, the head of the queue. If the queue is empty, it returns null. Default Implementation: It is abstract in AbstractQueue, so concrete classes must implement it.
+ element(): This method retrieves (but does not remove) the head of the queue, throwing a NoSuchElementException if the queue is empty. Default Implementation: return peek(); (calls peek() method).

AbstractQueue doesn't introduce any unique methods
 
### Deque

Deque is an interface it's name stands for "double-ended queue" — a linear collection that allows insertion and removal from both ends, i.e., head and tail.
It extends the Queue<E> interface, but unlike a regular queue (which is FIFO: First-In-First-Out), a deque can behave like:
+ A queue → add at end, remove from front (offerLast(), pollFirst())
+ A stack → add/remove from the front (push(), pop())

A Deque (pronounced "deck") is a queue where you can insert and remove elements from both the front (head) and the rear (tail) of the queue. This is different from a regular queue (FIFO), where elements can only be added at the end and removed from the front.

In a Deque:
- Insertions can happen at both ends: addFirst() for the front and addLast() for the rear.
- Removals can happen at both ends: removeFirst() for the front and removeLast() for the rear.
- Peek operations: You can also peek at the first or last element without removing it, with peekFirst() and peekLast().

There are two popular implementations of Deque in Java:

#### ArrayDeque
It's backed by a resizable array. Internally uses a circular array — which wraps around when the end is reached. Keeps track of a head index and tail index to know where to insert/remove. When full, it doubles the array size and copies elements in order (just like ArrayList does). Offers O(1) time for add/remove at both ends — unless resizing happens. Doesn’t allow null elements (they’re used as markers in the internal array).

```java
int head = 0; // index for front
int tail = 0; // index for end

// insert at front: decrement head (wrap if needed)
head = (head - 1 + elements.length) % elements.length;

// insert at end: add to tail, increment (wrap if needed)
tail = (tail + 1) % elements.length;
```

#### Another one is an ArrayList, but it is covered in more detail in a file dedicated to List specifically.

### BlockingQueue

The `BlockingQueue<E>` interface is part of the java.util.concurrent package and extends the `Queue<E>` interface. Its key feature is that it supports operations that wait for the queue to become non-empty when retrieving elements and for space to become available when storing elements. This makes it especially useful in producer-consumer scenarios, where one or more threads produce data and others consume it.

It introduces blocking methods like:
+ put(E e) – waits if necessary for space to become available.
+ take() – waits if necessary until an element becomes available.
+ offer(E e, long timeout, TimeUnit unit) and poll(long timeout, TimeUnit unit) – timed versions of offer and poll.

BlockingQueue is specifically designed for multithreaded environments. The term "blocking" refers to how the queue handles concurrent access by multiple threads:
+ If a thread tries to take() an element from an empty queue, it will block (wait) until another thread inserts an element.
+ If a thread tries to put() an element into a full queue (in bounded queues like ArrayBlockingQueue), it will block until space becomes available.
+ This makes BlockingQueue very useful for producer-consumer scenarios, where threads need to safely hand off work or data to each other without manually managing synchronization or locks.

-------------------------------

## Classes that implement and extend above mentioned Interfaces and Classes

### PriorityQueue
##### ⏰ Provides O(log n) time for insertion and removal due to its binary heap structure. It's slower than ArrayDeque or LinkedBlockingQueue for simple FIFO tasks but ideal when element priority matters.

PriorityQueue in Java is a class that implements the Queue interface and provides an ordered collection where the elements are ordered according to their natural ordering or by a Comparator provided at the time of queue construction. Unlike a regular queue, where elements are processed in the order they were inserted (FIFO), a PriorityQueue orders its elements based on priority, with the highest (or lowest) priority element being served first. It implements Queue, but doesn't extend any class. It implements Queue and extends AbstractQueue.

PriorityQueue uses a binary heap (specifically a min-heap by default) to store its elements. A binary heap is a complete binary tree, where each node has a higher (or lower, depending on the heap type) priority than its children. Here are the key points on how it works:
+ Heap Structure: The elements are stored in an array-backed binary heap, where the root element is always the one with the highest priority (for a min-heap) or lowest priority (for a max-heap). The heap property is maintained, meaning the parent node is always less than (in a min-heap) or greater than (in a max-heap) its child nodes.
+ Insertion (offer): When a new element is added (offer(E e)), it is initially inserted at the end of the heap array. After insertion, the heap property may be violated, so the element "bubbles up" (via the siftUp process) to restore the heap order. This operation is O(log n), where n is the number of elements in the queue.
+ Removal (poll): The highest priority element (the root of the heap) is removed first (poll()). To remove the root, the last element in the heap replaces it, and then the heap property is restored by "bubbling down" (via the siftDown process). This operation is also O(log n).
+ Peek: The peek() method simply returns the root element (highest priority element), without removing it. It’s an O(1) operation.
+ Ordering: If no Comparator is provided when the PriorityQueue is created, the queue assumes that the elements are comparable (i.e., they implement Comparable<E>). This means elements are ordered according to their natural ordering. If a Comparator is provided, it is used to compare elements when ordering them in the queue.

Key Characteristics
+ Unbounded Size: A PriorityQueue grows dynamically as needed, with no pre-set size limit.
+ No Capacity Limit: The queue expands automatically, so you don’t need to specify an initial capacity unless you want to optimize for a known number of elements.
+ Not Thread-Safe: PriorityQueue is not synchronized, meaning it is not safe to use concurrently across multiple threads unless external synchronization is provided.
+ Custom Comparators: It allows custom ordering of elements through the Comparator interface, giving flexibility in defining the queue’s priority logic.

Example usage:
```java
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] args) {
        // Default PriorityQueue (min-heap)
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.offer(5);
        pq.offer(3);
        pq.offer(8);
        pq.offer(1);

        while (!pq.isEmpty()) {
            System.out.println(pq.poll()); // This will print: 1, 3, 5, 8
        }
    }
}
```
In the example above, the PriorityQueue automatically orders the elements so that the smallest element (according to the natural ordering of integers) is always at the front. When elements are polled from the queue, they come out in ascending order.

When to Use PriorityQueue?
+ PriorityQueue is ideal when you need to process elements in a specific order based on priority, and the order can change dynamically (i.e., you may insert and remove elements with varying priorities).
+ Use cases: Task scheduling, simulations, Dijkstra's algorithm (shortest path), or anywhere that a priority-based queue is required.
+ Not ideal for: Simple FIFO queue processing, where the order of elements is based strictly on arrival time.

In summary, PriorityQueue offers a way to process elements based on priority, with efficient insertion and removal due to its use of a binary heap. However, it’s not thread-safe, and it doesn’t guarantee any particular order of elements with the same priority unless specified by a Comparator.

##### What is a binary heap.
It's kina (in a very broad sense) similar to Red-Black Tree, because it gets represented as a tree. But the only relation between parent and children elements of this tree is that the parent of two children (btw tree gets filled from left to right, and there can be no right-side child without left-side one, but there can be a single left-side child) has to be smaller (in case of min heaps) or equal to its child, one, and the oter, (but not both at the same time) or larger or equal (in case of max-heaps). **The heap sacrifices full order (like sorting) in exchange for fast access to the most important element according to some priority**. The heap isn't orded outside of this basic rule so if 1 is the smallest number its children can be 2 and 270, heap serves very little value outside of getting the most prioritized item, and it will always give you the most prioritize item as it employes self balancing to move the smallest / largest item to the top. 

One thing to know about binary heap that unlike RBT, it's represented as an array. Take a look at the following Tree representation:
```mardown
        2
       / \
      3   4
     / \   \
    5   10  8

```
And now look at how it is actually stored in array form:
```makefile
Index:    0   1   2   3   4   5
Value:    2   3   4   5   10  8
```

### ArrayDeque
##### ⏰ ArrayDeque offers O(1) time complexity for insertion and removal at both ends, making it generally faster than LinkedList for deque operations due to better cache locality and no node allocation overhead. Compared to ArrayList, it performs similarly in array resizing but is optimized for queue-like access patterns. Overall, it is one of the fastest choices for stack or queue use cases where thread safety is not required. It's faster than the other queues here for non-blocking, single-threaded scenarios.

ArrayDeque is a class that implements the Deque interface (a type of queue that allows insertion and removal from both ends). It is a dynamic array-based implementation of a double-ended queue (deque), meaning it provides the functionality of adding and removing elements from both the front and back of the queue efficiently. It's a dynamic array-based deque implementation that provides fast insertion and removal operations at both ends. It automatically resizes its internal array as needed and is highly efficient for stack and queue operations. It does not allow null elements and is not thread-safe by default. It is often a better choice than LinkedList for most use cases because of its simpler, array-based structure, which results in less overhead.

Key Features:
+ Resizable Array: Internally, ArrayDeque uses a resizable array to store the elements. **Unlike ArrayList, which resizes when the array exceeds its capacity, ArrayDeque resizes in a way that ensures elements can be added and removed efficiently from both ends.**
+ No Capacity Limit: ArrayDeque does not have a fixed capacity. It automatically grows when needed (i.e., when the array is full), but unlike LinkedList, it does not have the overhead of node-based pointers.
+ Non-Thread-Safe: Unlike some other queue classes like LinkedBlockingQueue, ArrayDeque is not thread-safe by default. This means that if multiple threads are modifying it, synchronization must be handled externally.
+ Faster than LinkedList: **ArrayDeque generally performs better than LinkedList for most operations (especially for stack and queue operations)**. This is because ArrayDeque uses an array internally, which avoids the overhead of managing nodes in a doubly linked list.
+ No Null Elements: ArrayDeque does not allow null elements, unlike some other collections, which can store null values. This helps prevent ambiguity when performing operations on the deque.

How It Works Behind the Scenes:
+ ArrayDeque uses an array to hold its elements. It maintains two pointers or indices (head and tail) that indicate where the next element will be added or removed. **The main goal of the ArrayDeque is to maintain constant-time operations (O(1)) for adding or removing elements at either end.**
+ When elements are added to the front or back of the deque, the array dynamically resizes if necessary. **The resizing happens in such a way that it provides amortized constant time for these operations.**
+ **If the deque becomes full, ArrayDeque doubles its array size.** Similarly, **when the size of the deque decreases, the underlying array may shrink to optimize memory usage.**
+ To keep track of the elements in the array, ArrayDeque wraps around its array in a circular fashion. This means when elements are removed or added beyond the boundaries of the array (either at the start or the end), the array is conceptually "wrapped" around to the other side.

Performance Considerations:
+ Time Complexity: Most operations like adding or removing elements at either end (addFirst(), addLast(), removeFirst(), removeLast()) are O(1), which makes ArrayDeque very efficient. Resizing the underlying array (when it becomes full or shrinks) happens infrequently and thus has an amortized O(1) time complexity.
+ Memory Usage: ArrayDeque uses an array, so it has a constant memory overhead of storing the elements. When resizing, the time complexity is O(n) for the resize operation, but this happens infrequently.
+ Thread Safety: ArrayDeque is not synchronized by default. If thread-safety is needed, external synchronization or a different thread-safe collection (like ConcurrentLinkedDeque) should be used.

Use Cases:
+ Efficient Queue Operations: If you need a queue that allows for adding and removing elements from both ends efficiently, ArrayDeque is a great choice. It is faster than LinkedList and more flexible than a simple array.
+ Stack and Queue Operations: You can use ArrayDeque as both a stack (LIFO) and a queue (FIFO). Since ArrayDeque allows constant-time operations at both ends, it's a good choice for these types of data structures.

### ArrayBlockingQueue
##### ⏰ Provides O(1) time for insertion and removal, but uses a single lock for both operations, which can lead to contention under heavy multithreading. It's faster than LinkedBlockingQueue in low-contention, small-capacity scenarios due to its array backing (so when there are just a couple of threads).

`ArrayBlockingQueue<E>` is a bounded, blocking queue backed by a fixed-size array, meaning its capacity is set at creation and cannot grow. It follows FIFO (first-in-first-out) order and is thread-safe thanks to internal locks used for coordinating access between producers and consumers.

Because it’s array-backed, it offers predictable performance and memory use, with constant-time offer(), poll(), peek(), and size() operations under most conditions. However, unlike LinkedBlockingQueue, it does not allow for dynamic resizing, making it ideal for situations where a known, fixed capacity is desired for resource control.

### LinkedBlockingQueue
##### ⏰ Just as ArrayBlockingQueue offers O(1) time complexity but uses separate locks for put and take, making it perform better than ArrayBlockingQueue under high-concurrency conditions. Slightly slower for small or single-threaded workloads due to overhead of node management (so it's better for a lot of threads (like 10s or 100s)).

LinkedBlockingQueue is a thread-safe, optionally bounded implementation of the BlockingQueue interface that uses a linked-node structure internally.
+ It can be bounded (with a specified capacity) or unbounded (defaulting to Integer.MAX_VALUE).
+ Compared to ArrayBlockingQueue, it generally provides better throughput under high concurrency because it uses separate locks for put and take operations (whereas ArrayBlockingQueue uses a single lock).
+ Internally, it uses a linked list of nodes, which allows dynamic memory usage without needing to resize arrays.
