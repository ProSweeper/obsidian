---
tags:: algorithms, graph
site:: structy
---
### Has Path 
Write a function, _hasPath_, that takes in an object representing the adjacency list of a directed acyclic graph and two nodes (_src_, _dst_). The function should return a boolean indicating whether or not there exists a directed path between the _source_ and _destination_ nodes.

```javascript
const hasPath = (graph, src, dst) => {
  const queue = [src];
  while (queue.length > 0) {
    const curr = queue.shift();
    if (curr === dst) return true;
    
    for (let neighbor of graph[curr]) {
      queue.push(neighbor);
    }
  }
  return false
};
```

#### Approach: 
- visualize the adjacency list with a drawing 
- make sure the graph is not cyclic so there won't be any infinite loops
	- if there was a cyclic graph then we could keep track of the nodes visited to avoid infinite loops
- We can use a BFS or a DFS to solve this problem, I selected BFS
- We then implement the BFS Traversal Algorithm: [[Algorithms to Know#^558da1]] with a modification
	- instead of printing out the node, we simply check to see if the curr node is equal to the destination node in the function arguments 

### Undirected Path
Write a function, _undirectedPath_, that takes in an array of edges for an undirected graph and two nodes (_nodeA_, _nodeB_). The function should return a boolean indicating whether or not there exists a path between _nodeA_ and _nodeB_.

#### Approach
- We should convert the array of edges into a more favorable format, an adjacency list
- Traversal Algorithms work best with adjacency lists
- To create the adjacency list we need to initialize an object (or a map) to house the nodes and their neighbors 
- We then iterate through the edge arrays and create keys for each of the edges that appear and initialize them with an empty array. 
- then when 2 nodes appear in an edge they are considered neighbors and we append them to each others list of neighbors in the map
- now that we have the list we can draw out our graph
- 