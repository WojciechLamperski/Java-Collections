

### Queue
The Queue<E> interface in Java is part of the java.util package and extends the Collection<E> interface. It represents a FIFO (First-In-First-Out) data structure, where elements are added at the rear and removed from the front.
It is commonly used for task scheduling, buffering, and breadth-first search (BFS) algorithms.

Key Features of Queue
- FIFO Order: Elements are processed in the order they are added.
- Limited vs. Unbounded: Some queues have a fixed size (e.g., ArrayBlockingQueue), while others grow dynamically (e.g., LinkedList or PriorityQueue).
- Blocking and Non-Blocking Queues: Blocking queues like LinkedBlockingQueue wait when the queue is empty or full.
- Non-blocking queues like LinkedList return special values (null or false) when an operation fails.
- Priority-Based Processing: PriorityQueue orders elements based on natural ordering or a comparator.
- Thread-Safety: Some implementations, like ConcurrentLinkedQueue, are designed for multi-threaded environments.

Anti-Use Cases
❌ Random Access – Lists (ArrayList, LinkedList) are better.
❌ LIFO Behavior – Use Deque (e.g., ArrayDeque, Stack) instead.
❌ Fast Lookup by Value – Set or Map would be better choices.

Use Cases of Queue
✔ Task Scheduling – Thread pools, event processing.
✔ Producer-Consumer Problems – BlockingQueue helps in managing concurrent data.
✔ Breadth-First Search (BFS) – Queues help traverse trees and graphs.
✔ Message Queues – Background processing and messaging systems (e.g., Kafka).

##### Queue Implementations:

- LinkedList<E>
Implements Queue<E> and Deque<E>, doubly linked list-based.
Allows null elements.
Suitable for simple FIFO behavior.
Uses more memory due to extra node pointers, better for frequent insertions/removals.

- PriorityQueue<E>
Not strictly FIFO; elements are ordered based on natural ordering or a Comparator.
Good for task scheduling where priority is needed.
Best for priority-based tasks, but slower than FIFO queues for simple insert/removal.

- ArrayDeque<E>
Faster than LinkedList for queue operations.
Used for both FIFO (Queue) and LIFO (Stack) operations.
Does not allow null.
Faster than LinkedList, avoids extra memory for node pointers, but resizing can be costly.

- ConcurrentLinkedQueue<E>
Thread-safe, lock-free non-blocking queue.
Uses CAS (Compare-And-Swap) operations for performance in multi-threaded environments.
Lock-free, ideal for high-concurrency environments. Slight overhead due to atomic operations.

- BlockingQueue<E> (Sub-interface of Queue)
Used in multi-threading, blocking when empty or full.
Implementations:
+ LinkedBlockingQueue (linked list-based, optional capacity)
+ ArrayBlockingQueue (array-based, fixed capacity)
+ PriorityBlockingQueue (like PriorityQueue but thread-safe)
+ DelayQueue (elements expire after a delay)
Best for multi-threaded producer-consumer models. However, blocking operations can introduce thread contention.
