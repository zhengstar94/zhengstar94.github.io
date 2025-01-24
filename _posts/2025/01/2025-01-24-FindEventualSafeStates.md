---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "802. Find Eventual Safe States"
date: "2025-01-24"
tags: Medium
categories:
  - "LeetCode DFS"
---


- There is a directed graph of `n` nodes with each node labeled from `0` to `n - 1`. The graph is represented by a **0-indexed** 2D integer array `graph` where `graph[i]` is an integer array of nodes adjacent to node `i`, meaning there is an edge from node `i` to each node in `graph[i]`.
- A node is a **terminal node** if there are no outgoing edges. A node is a **safe node** if every possible path starting from that node leads to a **terminal node** (or another safe node).
- Return *an array containing all the **safe nodes** of the graph*. The answer should be sorted in **ascending** order.

**Example 1**

```
Input: graph = [ [ 1,2],[2,3],[5],[0],[5],[],[ ] ]
Output: [2,4,5,6]
Explanation: The given graph is shown above.
Nodes 5 and 6 are terminal nodes as there are no outgoing edges from either of them.
Every path starting at nodes 2, 4, 5, and 6 all lead to either node 5 or 6.
```

**Example 2**

```
Input: graph = [ [ 1,2,3,4],[1,2],[3,4],[0,4],[ ] ]
Output: [4]
Explanation:
Only node 4 is a terminal node, and every path starting at node 4 leads to node 4.
```

## Method 1

```tex
【O(V + E) time | O(V) space】
```

```java
package Leetcode.DFS;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/24
 */
public class FindEventualSafeStates {

    /**
     * Array to keep track of node states:
     * 0 = unvisited
     * 1 = currently being visited (in the current DFS path)
     * 2 = confirmed safe node
     */
    private static int[] visited;
    
    public static List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        visited = new int[n];
        List<Integer> result = new ArrayList<>();

        // Iterate through each node in the graph to check if it's safe
        for (int i = 0; i < n; i++) {
            if (dfs(i, graph)) {
                result.add(i);
            }
        }

        return result;
    }
    
    private static boolean dfs(int node, int[][] graph) {
        // Case 1: If we encounter a node that's currently being visited,
        // we've found a cycle, and all nodes in this cycle are unsafe
        if (visited[node] == 1) {
            return false;
        }

        // Case 2: If we encounter a node that's already been confirmed safe,
        // we can return true without further exploration
        if (visited[node] == 2) {
            return true;
        }

        // Mark the current node as being visited (in the current DFS path)
        // This helps in cycle detection
        visited[node] = 1;

        // Explore all neighboring nodes
        // If any path from the current node leads to an unsafe node,
        // the current node is also unsafe
        for (int next : graph[node]) {
            if (!dfs(next, graph)) {
                return false;  // Found an unsafe path
            }
        }

        // If we reach here, all paths from this node are safe
        // Mark the node as a confirmed safe node
        visited[node] = 2;
        return true;  // Node is safe
    }
    
    public static void main(String[] args) {
        // Test Case 1: Graph with multiple safe nodes
        int[][] graph1 = { { 1,2},{2,3},{5},{0},{5},{},{ } };
        System.out.println("Test Case 1 Result: " + eventualSafeNodes(graph1)); // Expected output: [2,4,5,6]

        // Test Case 2: Graph with only one safe node
        int[][] graph2 = { { 1,2,3,4},{1,2},{3,4},{0,4},{ } };
        System.out.println("Test Case 2 Result: " + eventualSafeNodes(graph2)); // Expected output: [4]

        // Test Case 3: Graph where all nodes are safe
        int[][] graph3 = { { },{0,2,3,4},{3},{4},{ } };
        System.out.println("Test Case 3 Result: " + eventualSafeNodes(graph3)); // Expected output: [0,1,2,3,4]
    }
}

```





