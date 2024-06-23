---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Cycle In Graph"
date: "2023-03-19"
categories:
  - "Algorithm Graphs"
---

# Cycle In Graph [Medium]

- You're given a list of `edges` representing an unweighted, directed graph with at least one node. Write a function that returns a boolean representing whether the given graph contains a cycle.
- For the purpose of this question, a cycle is defined as any number of vertices, including just one vertex, that are connected in a closed chain. A cycle can also be defined as a chain of at least one vertex in which the first vertex is the same as the last.
- The given list is what's called an adjacency list, and it represents a graph. The number of vertices in the graph is equal to the length of `edges`, where each index `i` in `edges` contains vertex `i`'s outbound edges, in no particular order. Each individual edge is represented by a positive integer that denotes an index (a destination vertex)in the list that this vertex is connected to. Note that these edges are directed, meaning that you can only travel from a particular vertex to its destination, not the other way around (unless the destination vertex itself has an outbound edge to the original vertex).
- Also note that this graph may contain self-loops. A self-loop is an edge that has the same destination and origin; in other words, it's an edge that connects a vertex to itself. For the purpose of this question, a self-loop is considered a cycle.
- For a more detailed explanation, please refer to the Conceptual Overview section of this question's video explanation.

**Sample Input**

```
matrix =
[
  [1,3], 
  [2,3,4], 
  [0], 
  [], 
  [2,5], 
  []
]
```

**Sample Output**

```
true
// There are multiple cycles in this graph: 
// 1)0->1->2->0
// 2)0->1->4->2->0
// 3)1->2->0->1
// These are just 3 examples; there are more.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> There are multiple ways to solve this problem, and they all make use of a depth-first-search traversal. </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> When traversing a graph using depth-first search, a back edge is an edge from a node to one of its ancestors in the depth-first-search tree, and a back edge denotes the presence of a cycle. How can you determine if a graph has any back edges? </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> To find back edges, you'll need to keep track of which nodes you've already visited and which nodes are ancestors of the current node in the depth-first-search tree. There are a few ways to do this,but one approach is to recursively traverse the graph and to keep track of which nodes have been visited in general and which nodes have been visited in the current recursion stack; you can do so with two separate data structures. If you reach a node that has an edge to a node that's already in the recursion stack, then you've detected a back edge, and there's a cycle in the graph.</strong></i>
</details>


<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Similar to the previous hint,you can also detect a back edge by performing a 3-color depth-first search. Each node is colored white to start; recursively traverse through the graph, coloring the current node grey and then calling the recursive traversal function on all of its neighbors. After traversing all the neighbors,color the current node black to signify that it's "done. "If you ever find an edge to a node that's grey, you've found a back edge,and there's a cycle in the graph. </strong></i>
</details>


<br>



## Method 1

```tex
【O(V+E)time∣O(E)space】
```

<br>


```tex
1.\ V\ is\ the\ number\ of\ nodes\\
2.\ E\ is\ the\ number\ of\ sides\
```

```java
package Graphs;

/**
 * @author zhengstars
 * @date 2023/03/18
 */
public class CycleInGraph2 {
    public static boolean cycleInGraph(int[][] edge) {
        int n = edge.length;
        
        // Used to record whether the node has been accessed
        boolean[] visited = new boolean[n];
        // Used to record the nodes visited in the current dfs search
        boolean[] dfsVisited = new boolean[n];
        for (int i = 0; i < n; i++) {
            // If the node has not been accessed, conduct a dfs search
            if(!visited[i]){
                if(dfs(edge,n,visited,dfsVisited)){
                    return true;
                }
            }
        }
        return false;
    }

    private static boolean dfs(int[][] edge, int node, boolean[] visited, boolean[] dfsVisited) {
        // Mark that the node has been accessed
        visited[node] = true;
        // Mark that the node has been accessed in the current dfs search
        dfsVisited[node] = true;

        for (int neighbor : edge[node]) {
            // If the node has not been accessed, perform a dfs search
            if(!visited[neighbor]){
                if(dfs(edge,neighbor,visited,dfsVisited)){
                    return true;
                }
            }else if(dfsVisited[neighbor]){
                // If the node has been accessed and has been accessed in the current dfs search, there is a ring
                return true;
            }
        }

        // Reset the state of the node because other dfs searches may also need to access the node
        dfsVisited[node] = false;
        return false;
        
    }
}

```

