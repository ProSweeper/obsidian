### Structure

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fa27db5-cb5d-4708-9be5-fed3a04af7ba/Untitled.png)

-   A link list is formed by nodes linked together in a chain
    -   each node has a value and a pointer to the next node in the list

### Node Class

-   a node class has 2 components:
    -   Data: Value you want to store in a node (like a value at a specific index of an array)
    -   Pointer: refers us to the next node in the list (essential for connectivity)

```jsx
class Node {
  constructor(data) {
    this.data = data;
    this.nextElement = null;
  }
}
```

### Linked List Class

-   Linked list is a collection of node objects
-   to keep track of the list we need a pointer to the first node in the list
-   this is the head node. it does not contain any data, it just points to the start of the list

```jsx
class LinkedList {
 constructor(){
   // head will be at the top of the list
   this.head = null; 
  }
}
```

-   We initialize the head pointer to null since the linked list starts off empty

### Basic Linked List Operations

-   `insertAtTail(data)` - inserts an element at the end of the linked list
-   `insertAtHead(data)` - inserts an element at the start/head of the linked list
-   `delete(data)` - deletes an element with your specified value from the linked list
-   `deleteAtHead()` - deletes the first element of the list
-   `search(data)` - searches for an element in the linked list
-   `isEmpty()` - returns true if the linked list is empty
    -   helper function that will be useful for other functions
    -   to check if a list is empty we just need to see if the head node value is null

### List Insertion

-   3 types of insertion:
    
    -   insert at head
    -   insert at tail
    -   insert at nth index
-   Insert at head:
    
    -   we want to insert a new element as the first element of the list
        -   head will point to the newly added node. this new node will point to either the node that was the previous head or null if it is the first element
-   Insert at Tail
    
    -   we want to add a new element to the end of the list
        -   original tail will be pointing to null, we set the original tail to point to our new tail node. And then we set the new tail node to point to null
-   Insert at nth index
    
    -   traverse list looking for the nth element
    -   when we find it we assign the new nodes next element to the nth nodes next element
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aeda281b-4560-4e01-a50a-56f14aaa8457/Untitled.png)
    
    -   then we assign the nth nodes next element to be the new node
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/30a7bb64-e8a1-4a8c-8f7d-3dcc9913e6f8/Untitled.png)
    

### List Deletion

-   3 types of deletion
    
    -   Deletion at the head
    -   Deletion by value
    -   Deletion at the tail
-   Deletion at the head
    
    -   deletes the first node, if the list is empty then we do nothing
        
    -   access the head of the linked list and point it to the second node
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/502122b8-72aa-4424-8103-834b368b23a1/Untitled.png)
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bb630954-84d1-48b7-9c21-485b44777729/Untitled.png)
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/107a55c0-efe1-4906-98e8-b3d07fdb5f79/Untitled.png)
        
        ```jsx
        LinkedList.prototype.deleteAtHead = function() {
          //if list is empty, do nothing
          if (this.isEmpty()) {
            return this;
          }
          //Get the head and first element of the list
          let firstElement = this.head;
        
          //If list is not empty, link head to the nextElement of firstElement
          this.head = firstElement.nextElement;
        
          return this;
        }
        ```
        
-   Deletion by value
    
    -   traverse the list to look for the value
    -   if the value is found, set the previous nodes pointer to the node after the one we are deleting
    -   then delete the value
    
    ```jsx
    LinkedList.prototype.deleteVal = function(value) {
      
      //if list is empty return false
      if (this.isEmpty()) {
        return false;
      }
    
      //else get pointer to head
      let currentNode = this.head;
      // if first node's is the node to be deleted, delete it and return true
      if (currentNode.data == value) {
        this.head = currentNode.nextElement;
        return true;
      }
    
      // else traverse the list
      while (currentNode.nextElement != null) {
        // if a node whose next node has the value as data, is found, delete it from the list and return true
        if (currentNode.nextElement.data == value) {
          currentNode.nextElement = currentNode.nextElement.nextElement;
          return true;
        }
        currentNode = currentNode.nextElement;
      }
      //else node was not found, return false
      return false;
    }
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d7d15f1e-c610-4d58-ba53-ace6c25d29fa/Untitled.png)
    
-   Deletion at the tail
    
    -   traverse to the end of the list and delete it by setting the second to last nodes pointer to null
    
    ```jsx
    LinkedList.prototype.deleteAtTail = function() {
      // check for the case when linked list is empty
      if (this.isEmpty()) {
        return this;
      }
      //if linked list is not empty, get the pointer to first node
      let firstNode = this.head;
      //check for the corner case when linked list has only one element
      if (firstNode.nextElement == null) {
        this.deleteAtHead();
        return this;
      }
      //otherwise traverse to reach second last node
      while (firstNode.nextElement.nextElement != null) {
        firstNode = firstNode.nextElement;
      }
      //since you have reached second last node, just update its nextElement pointer to point at null, skipping the last node
      firstNode.nextElement = null;
      return this;
    }
    ```
    

### Union of Linked Lists

-   Given 2 lists A and B; the union is the list that contains elements or objects that belong to either A, or to B, or both

### Intersection of Linked Lists

-   Given 2 lists A and B, the intersection is the largest list, which contains all the elements that are common to both sets