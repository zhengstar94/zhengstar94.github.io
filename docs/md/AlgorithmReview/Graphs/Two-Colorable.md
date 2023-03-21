# Two-Colorable

- You're given a list of `edges` representing a connected, unweighted, undirected graph with at least one node. Write a function that returns a boolean representing whether the given graph is two-colorable.
- A graph is two-colorable (also called bipartite) if all of the nodes can be assigned one of two colors such that no nodes of the same color are connected by an edge.
- The given list is what's called an adjacency list, and it represents a graph. The number of vertices in the graph is equal to the length of `edges`, where each index `i` in `edges` contains vertex `i`'s siblings, in no particular order. Each individual edge is represented by a positive integer that denotes an index in the list that this vertex is connected to. Note that this graph is undirected, meaning that if a vertex appears in the edge list of another vertex, then the inverse will also be true.
- Also note that this graph may contain self-loops. A self-loop is an edge that has the same destination and origin; in other words, it's an edge that connects a vertex to itself. Any self- loop should make a graph not 2-colorable.

**Sample Input**

```
edges =[
	[1, 2],
	[0, 2], 
	[0, 1]
]
```

**Sample Output**

```
		  // Nodes 1 and 2 must be different colors than node 0.
		  // However, nodes 1 and 2 are also connected, meaning t they must also have different colors,
False // which is impossible with only 2 available colors.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try starting by choosing a random node and assigning it a color.From here, can you tell what colors any other nodes must have? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> From a given node, assign each sibling node the opposite color,then continue through the graph using BFS or DFS. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> If you ever encounter a sibling that is already marked as the wrong color, then there cannot be a solution. </strong></i>
</details>

<br>

