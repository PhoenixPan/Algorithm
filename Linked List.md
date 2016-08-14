

##Single Linked List
All operations are O(1) except search/insert kth/remove tail (slow as it has to keep the tail pointer correct)/remove kth, which are O(N).

##Stack
Last-In-First-Out (LIFO) 
All operations are O(1).

##Queue
Queue is basically a protected Single Linked List where we can only search the head item (peek), insert new item to the tail (enqueue), and remove existing item from the head (dequeue).

First-In-First-Out (FIFO)
All operations are still O(1) as we do not use the slow O(N) remove tail operation.


##Doubly-linked List
Each vertex contains two links that refer to the previous and the next vertex.

All operations are O(1) except search/insert kth/remove kth, which are O(N). Notice that the remove tail operation is now O(1) in Doubly Linked List.

##Double-ended Queue (Deque)
Generalizes a queue, add or remove from either head or tail. Simply a protected Doubly-linked List.
All operations are O(1).


#####Question 1: What are the computational complexity of these structures?
#####Question 2: In which of these structures, edges are directed? Which are not?


