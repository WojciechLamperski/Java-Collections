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

