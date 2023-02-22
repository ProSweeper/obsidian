---
tags:: algorithms
---
### Depth First Search
![[Graph-ex.excalidraw]]
#### Iterative

```javascript
function depthFirstPrint(graph, source) {
	const stack = [ source ];

	while (stack.length > 0) {
		const curr = stack.pop()
		console.log(curr);
		
		for (let neighbor of graph[curr]) {
			stack.push(neighbor)
		}
	}
}
const graph = {
	a: ['b', 'c'],
	b: ['d'],
	c: ['e'],
	d: ['f'],
	e: [],
	f: []
};
depthFirstPrint(graph, 'a'); // acebdf
```
##### Explanation 
![[07-Graphs#^a2aa38]]

#### Recursive 

```javascript
function depthFirstPrint(graph, source) {
	console.log(source)
	for (let neighbor of graph[source]) {
		depthFirstPrint(graph, neighbor);
	}
}
const graph = {
	a: ['b', 'c'],
	b: ['d'],
	c: ['e'],
	d: ['f'],
	e: [],
	f: []
};
depthFirstPrint(graph, 'a'); // acebdf
```

### Breadth  First Search

^558da1

#### Iterative
```javascript
function breadthFirstPrint(graph, source) {
	const queue = [ source ];

	while (queue.length > 0) {
		const curr = queue.unshift()
		console.log(curr);
		
		for (let neighbor of graph[curr]) {
			queue.push(neighbor)
		}
	}
}
const graph = {
	a: ['b', 'c'],
	b: ['d'],
	c: ['e'],
	d: ['f'],
	e: [],
	f: []
};
breadthPrint(graph, 'a'); // abcdef
```