	You can know more about any collection by just typing: what are the special traits of _insertcollection_ in chatgpt.

Array:
Array -> fixed size (not dynamic), Contiguous Memory Allocation (values are saved in memory locations that are right next to each other), Performance for random access of values is O(1) due to contiguous memory allocation, multidimensional arrays, for each loops (implements IEnumerable interface). Type safe which kills unboxing and turns runtime errors into compile time errors.

2d arrays are also stored in contiguous memory allocation fashion. In memory, the last number of the first row is stored right next to the first element of the second row and so on.

Collections were created because arrays were fixed and not dynamic. So collections are all dynamic = increase their size as needed.


Non generic Collections:
Garbage. Not type safe which leads to a shit ton of boxing and unboxing (it is actually required to happen) and execution time erros. 

Every element is implicitly type casted to object data type.

Should only be used when you literally have no other choice, which is when you need to store elements from a bunch of different data types. Even do a lot of the time you will be able to create a user defined class that will be able to hold all this elements of different that types as properties/data fields/behavior etc. If creating a user defined class solves the problem you shouldn't use generic collections.

In other words you only use this shit if you really cannot put everything that is relevant to your item/element/key/value/etc into a single data type.

Should be avoided at all costs. Considered to obsolete by all C# developers.


Generic Collections:

The generic part of generic collections allows type safety to be achieved therefore they are just better.

7 Main Generic Collections:
1. `List<T>` -> List stores elements in sequential order, allowing duplicate elements and supports indexed access.. Elements stored contiguously in memory.

2. `Dictionary<Tkey, Tvalue`> -> Dictionary stores key-value pairs, with unique keys. Use Dictionary when you require a “lookup table” type of structure. Keys have to be unique and not null. The keys are not sorted, very fast operations.

3. `Stack<T>` -> elements are pushed on top of one another and popped in a LIFO fashion (Last in First out).
			only allows the top element to be accessed and modified. unique stack operations are all O(1)/
			
4. `Queue<T>` -> simulates a queue (fila) where elements are enqued (entrando an fila) and dequeud (saindo da fila). Who goes out the queue first is who got there first (FIFO). Only the first element can be accessed and modified, the last element can only be added to the queue and nothing else. All unique operations are O(1).

5. `HashSet<T>` -> A HashSet is **a collection that holds unique elements in no particular order** ( O(1) complexity for adding, searching or removing). Set == unique,  All collections that are "set" have some Venn Diagram operations that will add or remove its own elements based on the elements of another collection that is passed as a parameter. Not indexable. Pretty much a dictionary with unique values in the value pair.

6. `SortedSet<T>` -> - Use `SortedSet` when you need a sorted [list](https://www.bytehide.com/blog/list-csharp) of unique items but with slower operations due to the tree structure. Not indexable. Underlying data structure is a Binary Tree (red black tree specifically), elements are always sorted based on the IComparable interface implementation (this is true for all sorted collections). O(log n) operations. 
-  Both sortedset and hashset Should be used when you do not need or doesn't want to allow elements to be accessed through index, key or randomly, when you want to enforce orderly access.

7. `LinkedList<T>` -> Data Structured: Doubly Linked List-> elements are nodes that store the value of the element, a pointer to the next element and a pointer to the previous method; the pointer are links to other elements...2pointers... Doubly Linked List.  Elements not contiguously allocated in memory. Faster insertion and deletion of elements then lists and arrays (because it just updates the value held by the links/nodes without shifting elements around). Should not be used for random access because you can only access a node by traversing nodes starting and the first or last node (expensive/inefficient). More memory efficient than arrays, specially if elements are added or removed constantly. Can only traverse a node iteratively or recursively (forwards or backwards). Can remove nodes the first and last nodes very efficiently. You can create a Circular linked list by making the last node of the Doubly Linked List connect point to the first node. Should be used when random access is not required and when the collection is being constantly modified.



Concurrent Collections

Generic collections that are thread safe: multiple threads can access the resource concurrently with no problems being generated. That is because they handle synchronization for us.

The concurrent collection have altered versions of the traditional collection operations  that make them thread safe.



Producer Consumer Pattern:

The producer-consumer pattern is a common concurrency pattern where two types of threads, producers and consumers, work together to accomplish a task. In this pattern, producers generate data and add it to a shared data store, and consumers retrieve and process the data from that store. This pattern helps decouple the producers and consumers, allowing them to work at their own pace without directly interacting.

The producer consumer pattern is achieved in practice by working around the concurrent collection capacity and calling methods that signal information from the producer threads to the consumers and vice-versa. Methods like: CompleteAdding() -> producers tell consumers that they are done adding elements to the collection, GetConsumingEnumerable() -> allow consumers for wait to elements to be added to the collection when the collection is null (empty), etc.

Concurrent Collections integrated well with the Task Parallel Library (TPL).


1. Concurrent Dictionary
2. Concurrent Stack
3. Concurrent Queue
4. Concurrent Bag -> kinda of a concurrent list except that the elements are not stored in a particular order because this collection is a concurrent collection. Should be used when a lot of elements are being added because it is optimized for add operations. Thread stealing -> each thread have their own local queue, threads can steal elements from another thread's queue for more efficiency.
5. Blocking Collection -> threads  can be block if calling Add() or Take() operations if the collection is full or empty. The main trait of this collection is the blocking of threads based on the count and capacity of the collection, made to facilitate the producer consumer pattern in multithreading. Capacity is bounded set a maximum limit for amount of elements in the collection.  The concurrent Stack, Queue and Bag collections all implement `IProducerConsumerCollection<T>` interface and therefore can be set as the underlying collection to store the elements, but by the default the `ConcurrentQueue<T>` is the underlying collection. `BlockingCollection<T>` supports cancellation through the use of `CancellationToken`. This allows for graceful termination of threads waiting on the collection in case of cancellation events.