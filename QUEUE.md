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

-------------------------------

## Classes that implement and extend above mentioned Interfaces and Classes

### PriorityQueue

PriorityQueue in Java is a class that implements the Queue interface and provides an ordered collection where the elements are ordered according to their natural ordering or by a Comparator provided at the time of queue construction. Unlike a regular queue, where elements are processed in the order they were inserted (FIFO), a PriorityQueue orders its elements based on priority, with the highest (or lowest) priority element being served first. It implements Queue, but doesn't extend any class.

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
