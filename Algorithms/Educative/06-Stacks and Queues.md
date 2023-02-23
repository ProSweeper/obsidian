-   Follows the Last in, First out (LIFO) ordering
    -   the last element added is the element on top
    -   first element added is on the bottom
-   Think of a stack of books, in order to get to a book in the middle you need to remove all the books on top of it. The last book you add to the pile is on top

### What are Stacks used for

-   backtrack to the previous task/state (for example in recursive code)
-   To store a partially completed task (for example when you are exploring 2 different paths on a graph from a point while figuring out the smallest path to the target)

### How do Stacks Work

### Stack Implementation

-   Stacks can be implemented using Arrays or Linked Lists
    
-   They must have the following 5 functions:
    
    -   `push(element)`
    -   `pop()`
    -   `isEmpty()`
    -   `getTop()`
    -   `size()`
-   A stack class would look like this in JS
    
    ```tsx
    class Stack {
    	constructor() {
    		this.items = [];
    		this.top = null;
    	}
    }
    const myStack = new Stack()
    ```
    

### Implementing `isEmpty()` `getTop()` and `size()`

```tsx
class Stack {
	constructor() {
		this.items = [];
		this.top = null;
	}
	getTop() {
		if (this.items.length === 0) return null;
		return this.top;
	}

	isEmpty() {
		return this.items.length === 0;
	}

	size() {
		return this.items.length;
	}
}
const myStack = new Stack()
```

### Implementing `push()` and `pop()`

```tsx
class Stack {
	constructor() {
		this.items = [];
		this.top = null;
	}
	...
	pop() {
		if (this.items.length !== 0) {
			if (this.items.length === 1) {
				this.top = null;
				return this.items.pop();
			} else {
				this.top = this.items[this.items.length - 2];
				return this.items.pop();
			}
		} else {
				return null;
		}
	}
}
const myStack = new Stack()
```

### Complexities of Stack Operations

### What is a Queue

-   Similar to a stack, another linear data structure that stores elements in a sequential manner
-   Queue uses a first in first out method (FIFO)
-   the element inserted first comes out first
    -   think of a line to buy a new gadget
-   Queues are a little harder to implement than a stack since we need to keep track of both ends
    -   elements are inserted at the back, and removed from the front

### What are Queues used for

-   searching and sorting algorithms
-   most operating systems also perform operations based on a priority queue
-   Generally use queues when we want to prioritze something over another or if a resource is shared between multiple devices

### How does a Queue Work

-   The functionality of a queue relies on the first two functions, the others are just helper functions

### Types of Queues

-   Linear Queue
-   Circular Queue
-   Priority Queue

### Circular Queue

-   Similar to a linear Queue with one exception: they are circular in structure
-   both ends are connected to form a circle
-   initially both the front and rear parts of the queue point to the same location
-   the eventually move apart as more elements are inserted into the queue
-   Used for:
    -   Simulation of objects
    -   Event Handling

### Priority Queue

-   all elements have a priority associated wiht them and are sorted such as that the most prioritized object appears in the front
-   widely used in most operating systems to determine which programs should be given more priority

### Queue Implementation

-   They can be represented with Arrays, linked lists, or even stacks
    
-   arrays are the most common use case
    
-   with typical arrays, the time complexity is O(n) when dequeuing an element
    
    -   this is because when an element is removed, the address of all the subsequent elements must be shifted by one. this makes it less efficient
-   With linked lists and doubly linked lists the operations become faster
    
-   A queue needs the following standard functions:
    
    -   `enqueue(element)`
    -   `dequeue()`
    -   `isEmpty()`
    -   `getFront()`
    -   `size()`
-   A queue class using doubly linked list would look like:
    
    ```tsx
    // node class
    class Node {
    
    constructor(item) {
          this.item = item;
          this.prev = null;
          this.next = null;
          }
    }
    // doubly linked list class
    class DoublyLinkedList {
      constructor() {
          this.length = 0;
          this.head = null;
          this.tail = null;
      }
    
      // Add node to the end of the list
      
      insertTail(item) {
        const newNode = new Node(item);
    
        if (this.length === 0) {
          this.head = newNode;
          this.tail = newNode;
        } else {
          this.tail.next = newNode;
    
          newNode.prev = this.tail;
    
          this.tail = newNode;
        }
    
        this.length++;
    
        return newNode;
      }
    
      // Remove node from the beginning of the list
      removeHead() {
        if (this.length === 0) {
          return null;
        }
    
        const nodeToRemove = this.head;
    
        if (this.length === 1) {
          this.head = null;
          this.tail = null;
        } else {
          this.head = nodeToRemove.next;
    
          this.head.prev = null;
          nodeToRemove.next = null;
        }
    
        this.length--;
    
        return nodeToRemove.item;
      }
    
      firstNode(){
    
        if (!(this.head == null)){
          return this.head.item;
        }else
          return null;
      }
    
      // Return list items
    
      toString() {
        const list = [];
        let currentNode = this.head;
    
        while (currentNode !== null) {
          list.push(JSON.stringify(currentNode.item));
          currentNode = currentNode.next;
        }
    
        return list.toString();
      }
    }
    
    class Queue {
      constructor() {
        this.items = new DoublyLinkedList();
      }
    
      isEmpty() {   
        return this.items.length == 0;
      }
    
      getFront() {
        if (!(this.isEmpty())) {
         return this.items.getHead(); 
      } else
         return null;
      }
    
      getTail() {
        if (!(this.isEmpty())) {
         return this.items.tailNode(); 
      } else
         return null;
      }
    
      size() {
        return this.items.length;
      }
      
      // Add an item to the queue (Time complexity: O(1))
      enqueue(element) {
        return this.items.insertTail(element);
      }
    
      // Remove an item from the queue (Time complexity: O(1))
      dequeue() {
        return this.items.removeHead();
      }
    }
    
    var myQueue = new Queue();
    ```
    
-   We consider the last element of a doubly linked list is the back of the queue (we enqueue elements here)
    
-   We consider the first element to be the front, and we will dequeue elements here)
    

### Complexities of Queue Operations

Operation

Time Complexity

isEmpty

O(1)

getFront

O(1)

getTail

O(1)

size

O(1)

enqueue

O(1)

dequeue

O(1)