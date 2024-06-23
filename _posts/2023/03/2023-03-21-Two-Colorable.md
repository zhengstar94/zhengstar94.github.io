---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Two-Colorable"
date: "2023-03-21"
categories:
  - "Algorithm Graphs"
---

# Two-Colorable [Medium]

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



## Method 1

```tex
【O(V+E)time∣O(V)space】

```

```tex
1.\ V\ is\ the\ number\ of\ nodes\\
2.\ E\ is\ the\ number\ of\ sides\
```

```java
package Graphs;

import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

/**
 * @author zhengstars
 * @date 2023/03/22
 */
public class TwoColorable3 {

    public static boolean isBipartite(int[][] graph) {
        // Check if the input matrix is null or empty
        if (graph == null || graph.length == 0) {
            return false;
        }
        // Mark array, used to record the color of each node, 0 means not colored yet, 1 means red, -1 means blue
        int[] colors = new int[graph.length];
        Arrays.fill(colors, 0);
        // Traverse each node
        for (int i = 0; i < graph.length; i++) {
            // If the node has not been colored yet, use BFS to color this node and its neighbors
            if (colors[i] == 0) {
                Queue<Integer> queue = new LinkedList<>();
                queue.offer(i);
                // Color the node as red
                colors[i] = 1;

                while (!queue.isEmpty()) {
                    int curr = queue.poll();
                    for (int neighbor : graph[curr]) {
                        // If the color of the neighbor node is the same as the current node's color, it does not meet the condition of a bipartite graph, return false
                        if (colors[neighbor] == colors[curr]) {
                            return false;
                        }
                        // If the neighbor node has not been colored yet, color it with a different color from the current node
                        if (colors[neighbor] == 0) {
                            colors[neighbor] = -colors[curr];
                            queue.offer(neighbor);
                        }
                    }
                }
            }
        }
        // If all nodes meet the condition of a bipartite graph after traversing, return true
        return true;
    }
}
```



## Method 2

```tex
【O(V+E)time∣O(V)space】
```

```tex
1.\ V\ is\ the\ number\ of\ nodes\\
2.\ E\ is\ the\ number\ of\ sides\
```

```java
package Graphs;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/03/22
 */
public class TwoColorable4 {
    public static boolean twoColorable(int[][] graph) {
        int n = graph.length;
        // initialize an array to store colors of vertices
        int[] colors = new int[n];
        // set all the colors to -1 (unvisited)
        Arrays.fill(colors, -1);

        // traverse all the vertices of the graph
        for (int i = 0; i < n; i++) {
            // if the current vertex has not been visited and it cannot be colored
            // with two colors, return false
            if (colors[i] == -1 && !dfs(graph, colors, i, 0)) {
                return false;
            }
        }
        // all the vertices can be colored with two colors, return true
        return true;
    }

    /**
     * dfs function to color the vertices
     * @param graph
     * @param colors
     * @param node
     * @param color
     * @return
     */
    private static boolean dfs(int[][] graph, int[] colors, int node, int color) {
        // if the current vertex has already been colored, return true if its
        // color matches with the input color; otherwise, return false
        if (colors[node] != -1) {
            return colors[node] == color;
        }
        // color the current vertex with the input color
        colors[node] = color;

        // recursively color all the neighboring vertices with the opposite color
        for (int next : graph[node]) {
            if (!dfs(graph, colors, next, 1 - color)) {
                return false;
            }
        }
        // all the vertices can be colored with two colors, return true
        return true;
    }
}

```

