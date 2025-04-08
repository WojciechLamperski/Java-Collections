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
├── PriorityQueue<E>   (implements Queue, unbounded, uses heap to order elements)  
├── ConcurrentLinkedQueue<E>   (implements Queue, non-blocking, uses linked nodes for thread safety)  
```
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

## Classes that implement and extend above mentioned Interfaces and Classes

### PriorityQueue
PriorityQueue in Java is a class that implements the Queue interface and provides an ordered collection where the elements are ordered according to their natural ordering or by a Comparator provided at the time of queue construction. Unlike a regular queue, where elements are processed in the order they were inserted (FIFO), a PriorityQueue orders its elements based on priority, with the highest (or lowest) priority element being served first.
