## What is a Graph

-   Graph data structure plays a fundamental role in several applications such as GPS, neural networks, peer to peer networks, search engine crawlers, garbage collection, and social networking websites

### Graph Structure

-   a graph is a set of nodes that are connected to each other in the form of a network
    
-   A graph has 2 basic components: Vertex and Edge
    
    -   **Vertex**: a collection of vertices forms a graph, vertices are similar to linked list nodes
    -   **Edge**: link between 2 vertices. can be uni directional or bi directional depending on the graph. an edge can also have a cost associated with it
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ecdfea94-231e-4f48-92c1-47a0c434f736/Untitled.png)
    

### Graph Terminologies

-   **Degree of a Vertex:** total number of edges connected to a vertex, there are two types of degerees:
    -   In-Degree: total number of incoming edges connected to a vertex
    -   Out-Degree: total number of outgoing edges connected to a vertex
-   **Parallel Edges**: 2 undirected edges are parallel if they have the same end vertices. two directed edges are parallel if they have the same origin and destination
-   ******************Self Loop******************: this occurs when an edge starts and ends at the same vertex
-   **************Adjacency:************** 2 vertices are considered adjacent if there is an edge connecting them directly
-   In the graph above, the In-Degree of both A and B is 1. The same for the Out-Degree. The In and Out degrees for C are both 2 since it contains a self loop

## Types of Graphs

-   Undirected
-   Directed

### Undirected Graph

-   In an undirected graph there is no notion of the direction to the edges.
-   the max amount of possible edges in an undirected graph that has _n_ vertices is ${\frac {n(n−1)}{2}}$

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d89b74a4-0529-4d91-ab83-aecd28a6825d/Untitled.png)

### Directed Graph

-   The edges of a directed graph are unidirectional, for the pair of (2,3) the only way to traverse is to go from 2-3, not the other way around
-   the max amount of possible edges in a directional graph is __n_(n-1)_*

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/edd13f70-f6c7-4fb0-8d1f-01d0ff54b778/Untitled.png)

## Representation of Graphs

There are 2 ways to represent a graph, Adjacency Matrix and Adjacency List

### Adjacency Matrix

-   A two dimensional matrix where each cell can contain a 0 or a 1. the row and column represent the vertices
-   If a cell contains a 1, there exists an edge between the corresponding vertices
-   eg: Matrix [0][1] = 1, then there is an edge that exists between vertex 0 and 1

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f7f5ff02-bb21-4f21-ba1f-1c921b2d2e4e/Untitled.png)

-   for a directed graph it is conventional to think of the rows as the source and the columns as the destination

### Adjacency List

-   An array of linked lists is used to store all the edges the graph
-   the size of the array is equal to the number of vertices
-   each index in this array represents a specific vertex in the graph
-   the entry at index `i` of the array contains a linked list containing the vertices that are adjacent to vertex `i`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa218cbf-b87d-406c-9d52-989d0729c2ac/Untitled.png)

-   the linked list at index 0 contains vertices 1 and 2 to represent the fact that the vertices 1 and 2 are adjacent to vertex 0
-   if a new vertex is added to the graph then it is simply added to the array as well

## Graph Implementation

-   The implementation will be based on the adjacency list model and the graphs will have directed edges

### Graph Class

-   contains 2 data members:
    
    -   total number of vertices in the graph
    -   an array of linked lists to store adjacent vertices
    
    ```tsx
    class Graph {
      constructor(vertices) {
        //Total number of vertices in the graph
        this.vertices = vertices;
        //Array that holds linked lists equal to the number of vertices in the graph
        this.list = [];
        //Creating a new linked list for each vertice of the graph
        var it;
        for (it = 0; it < vertices; it++) {
          let temp = new LinkedList();
          this.list.push(temp);
        }
      }
    
      addEdge(source, destination) {
        if (source < this.vertices && destination < this.vertices)
        //Since we are implementing a directed list, (0,1) is not the same as (1,0)
        this.list[source].insertAtHead(destination);
        //If we were to implement an undirected graph where (0,1)==(1,0), 
        //we would create an additional edge from destination to source too:
        //this.list[destination].insertAtHead(source);
      }
    
      printGraph() {
        console.log(">>Adjacency List of Directed Graph<<");
        var i;
        for (i = 0; i < this.list.length; i++) {
          process.stdout.write("|" + String(i) + "| => ");
          let temp = this.list[i].getHead();
          while (temp != null) {
            process.stdout.write("[" + String(temp.data) + "] -> ");
            temp = temp.nextElement;
          }
          console.log("null ");
        }
      }
    }
    
    let g = new Graph(4);
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 3);
    g.addEdge(2, 3);
    g.printGraph();
    
    // output:  
    >>Adjacency List of Directed Graph<<
    |0| => [2] -> [1] -> null 
    |1| => [3] -> null 
    |2| => [3] -> null 
    |3| => null
    ```
    
-   this is the foundation for our graph class.
    
-   the variable vertices contains an integer specifying the total number of vertices
    
-   the second component is an array which will act as our adjacency list
    
-   The functionality of a graph revolves around 2 functions:
    
    -   `addEdge(source, destination)`
        -   thanks to the constructor `source` and `destination` ar already stored as index of our array.
        -   this function simply inserts a destination vertex into the adacency linked list of the source vertex by running the following line of code: `this.list[source].insertAtHead(destination)`
        -   note that since we are implementing a directed graph, `addEdge(0,1)` is not the same as `addEdge(1,0)`
    -   `printGraph()`
        -   uses a simple nested loop to iterate through the adjacency list. each linked list is being traversed here

### Undirected Graph

-   in the case of an undirected graph, when we add an edge we also have to add an edge from the destination to the source. this makes the graph bidirectional

## Complexities of Graph Operations

### Time Complexities

-   in the table below, V means the total number of vertices, E means the total number of edges

### Adjacency List

-   Addition operations take constant time as we only need to insert at the head node of the corresponding vertex
-   Removing an edge takes O(E) time since in the worst case scenario, all the edges could be at a single vertex. this means we would have to traverse all E edges to reach the last one
-   removing a vertex takes O(V + E) time since we have to delete all its edges, then re-index the rest of the array one step back in order to fill the deleted spot

### Adjacency Matrix

-   Edge operations are performed in constant time as we only need to manipulate the value of a particular cell
-   Vertex operations are performed in O(V^2) time since we need to add rows and columns, then fill in the new cells

### Comparison

-   if your model frequently manipulates vertices then the adjacent list is more suitable
-   if you are dealing primarily with edges, the matrix is a better approach

## Bipartite Graph

-   special member in the graph family
    
-   vertices in this graph are divided into 2 disjointed parts in such a way that no 2 vertices in the same part are adjacent to each other
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e45af3d-1be9-4437-90eb-524a9b279e06/Untitled.png)
    
-   in the above examples each vertex can be placed into 2 groups (yellow and green) where no member of each group will have an edge connecting them
    
-   the bipartite graph is a type of k-partite where the value of k is 2
    
-   in a 5 partite graph we would have 5 disjointed sets, and members of a set would not be adjacent to each other
    

### Can a cyclical grpah be bipartite?

-   a cyclical graph is a graph where the edges form a cycle between the vertices
    
    -   if you traverse teh graph you will come back to a vertex you have already visited
-   yes, a cyclical graph can be bipartite if it has an even number of vertices.
    
-   But if a cyclical graph has an odd number of vertices, then it cannot be bipartite
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/93866b06-b882-4e05-82ad-775cfc7f6d45/Untitled.png)
    
-   Some popular types of bipartite graphs are:
    
    -   Star graph
    -   acyclic graph
    -   path graph

## Graph Traversal Algorithms

-   There are two basic techniques for traversing a graph, Breadth-First Search and Depth-First Search
-   any traversal needs a starting point, but a graph does not have a linear structure like an array.
-   this is where the concept of levels is introduced.
    -   Take any vertex as the starting point, this is the lowest level of your search
    -   the next level consists of all the vertices adjacent to your vertex
    -   a level higher would be the vertices adjacent to those

### Breadth-First Search

-   this algorithm grows breadth wise
    
-   all the nodes at a certain level are traversed before moving to the next level
    
-   the level wise expansion ensures that for any starting vertex, you can reach all others, one level at a time
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a40d67a4-61d5-457f-841b-34efdbdccf15/Untitled.png)
    
-   in the above graph the order of nodes searched would be 1-2-3-4-5-6
    

### Depth-First Search

-   this algorithm grows depth wise
    
-   starting from any node we keep moving to an adjacent node until we reach the furthest level
    
-   then we move back to the starting point and pick another adjacent node
    
-   then again, probe to the farthest level and move back
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d9c7823-275f-4043-bf49-f1ea09c38fbb/Untitled.png)
    
-   in the above graph the order would be 1-2-4-3-5-6
    

## Implementing Breadth First Search

-   Input: A graph in the form of an adjacency list
    
-   output: a string containing the vertices of the graph listed in the correct order of traversal
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fc0f260a-fe2b-437c-bbf7-421e5edf6f85/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce5bc25c-6499-4058-9238-edc987e49022/Untitled.png)
    
-   One implementation of BFS is as follows
    

```tsx
function bfsTraversal_helper(g, source, visited, obj) {
  
  
  //Create Queue(implemented in previous lesson) for Breadth First Traversal and enqueue source in it
  let queue = new Queue(g.vertices);
  queue.enqueue(source);
  visited[source] = true;
  //Traverse while queue is not empty
  while (queue.isEmpty() == false) {
    //Dequeue a vertex/node from queue and add it to result
    let current_node = queue.dequeue();
    obj.result += String(current_node);
    //Get adjacent vertices to the current_node from the list,
    //and if they are not already visited then enqueue them in the Queue
    let temp = g.list[current_node].getHead();
    while (temp != null) {
      if (visited[temp.data] == false) {
        queue.enqueue(temp.data);
        visited[temp.data] = true; //Visit the children
      }
      temp = temp.nextElement;
    }
  }
}

function bfsTraversal(g)
{
  if (g.vertices < 1){
    return null;
  }
  
  var obj = {result: ''}

  //An array to hold the history of visited nodes
  //Make a node visited whenever you push it into stack
  let visited = [];
  for (var x = 0; x < g.vertices; x++) {
    visited.push(false);
  }
    
  for (var i = 0; i < g.vertices; i++) {
    if (!visited[i])
      bfsTraversal_helper(g, i, visited, obj);
  }

  return obj.result;
}

let g1=new Graph(6);
g1.addEdge(1, 2);
g1.addEdge(1, 3);
g1.addEdge(2, 4);
g1.addEdge(2, 5);
console.log(bfsTraversal(g1, 0));
```

-   To implement a BFS we need to first create a variable to house the string we want to return, this will be an empty string to start
-   Then we create an array to keep track of the vertexes that have been visited
    -   the length of this array will be the same as the amount of vertices in the graph, each index will correspond to a vertex and be initialized as false (since we have not been there yet)
-   We then need a helper function to call on a vertex if it has not been visited
-   we need to create a helper function to be called on every vertex that has not been visited

### Implementation of Depth First Search

-   Input: A graph in the form of an adjacency list
-   output: a string containing the vertices of the graph listed in the correct order of traversal

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd380085-7d27-47d3-8ead-3c3c789ce4ec/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2e6f1991-5699-4e04-8156-426101bae1a6/Untitled.png)

-   Solution
  ```JavaScript
  function dfsTraversal_helper (g, source, visited, obj) {
  //Create Stack(Implemented in previous lesson) for Depth First Traversal and Push source in it
  let stack = new Stack(g.vertices);
  stack.push(source);
  visited[source] = true;
  //Traverse while stack is not empty
  while (stack.isEmpty() == false) {
    //Pop a vertex/node from stack and add it to the result
    let current_node = stack.pop();
    obj.result += String(current_node);
    //Get adjacent vertices to the current_node from the array,
    //and if they are not already visited then push them in the stack
    let temp = g.list[current_node].getHead();
    while (temp != null) {
      if (visited[temp.data] == false) {
        stack.push(temp.data);
         visited[temp.data] = true;
      }
      temp = temp.nextElement;
    } 
  }
}

function dfsTraversal(g)
{
  if (g.vertices < 1){
    return null;
  }
  
  var obj = {result: ''}

  //An array to hold the history of visited nodes
  //Make a node visited whenever you push it into stack
  var visited = [];
  for (var x = 0; x < g.vertices; x++) {
    visited.push(false);
  }
    
  for (var i = 0; i < g.vertices; i++) {
    if (!visited[i])
      dfsTraversal_helper(g, i, visited, obj);
  }

  return obj.result;
}
```