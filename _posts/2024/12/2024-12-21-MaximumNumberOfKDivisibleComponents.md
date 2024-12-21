---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "2872. Maximum Number of K-Divisible Components"
date: "2024-12-21"
tags: Hard
categories:
  - "LeetCode DFS"
---

- There is an undirected tree with `n` nodes labeled from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.
- You are also given a **0-indexed** integer array `values` of length `n`, where `values[i]` is the **value** associated with the `ith` node, and an integer `k`.
- A **valid split** of the tree is obtained by removing any set of edges, possibly empty, from the tree such that the resulting components all have values that are divisible by `k`, where the **value of a connected component** is the sum of the values of its nodes.
- Return *the **maximum number of components** in any valid split*.


**Example 1**

```
Input: n = 5, edges = [ [ 0,2],[1,2],[1,3],[2,4 ] ], values = [1,8,1,4,4], k = 6
Output: 2
Explanation: We remove the edge connecting node 1 with 2. The resulting split is valid because:
- The value of the component containing nodes 1 and 3 is values[1] + values[3] = 12.
- The value of the component containing nodes 0, 2, and 4 is values[0] + values[2] + values[4] = 6.
It can be shown that no other valid split has more than 2 connected components.
```

**Example 2**

```
Input: n = 7, edges = [ [ 0,1],[0,2],[1,3],[1,4],[2,5],[2,6 ] ], values = [3,0,6,1,5,2,1], k = 3
Output: 3
Explanation: We remove the edge connecting node 0 with 2, and the edge connecting node 0 with 1. The resulting split is valid because:
- The value of the component containing node 0 is values[0] = 3.
- The value of the component containing nodes 2, 5, and 6 is values[2] + values[5] + values[6] = 9.
- The value of the component containing nodes 1, 3, and 4 is values[1] + values[3] + values[4] = 6.
It can be shown that no other valid split has more than 3 connected components.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.DFS;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2024/12/21
 */
public class MaximumNumberOfKDivisibleComponents {
    private List<Integer>[] graph;  // Adjacency list representation of the tree
    private int[] values;          // Array storing the value of each node
    private int k;                 // The divisor
    private int result;           // Counter for number of valid components

    public int maxKDivisibleComponents(int n, int[][] edges, int[] values, int k) {
        // Initialize member variables
        this.values = values;
        this.k = k;
        this.result = 0;

        // Build undirected graph using adjacency list
        graph = new List[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }
        // Add edges to the graph (both directions as it's undirected)
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }

        // Start DFS from root node 0
        dfs(0, -1);

        return result;
    }

    private long dfs(int node, int parent) {
        // Initialize sum with current node's value
        long sum = values[node];

        // Process all neighbors of current node
        for (int child : graph[node]) {
            // Skip parent node to avoid cycles
            if (child != parent) {
                // Add the sum returned from child's subtree
                // If child's subtree formed a component, it will return 0
                sum += dfs(child, node);
            }
        }

        // Check if current subtree can form a valid component
        if (sum % k == 0) {
            result++;  // Found a new valid component
            return 0;  // Return 0 as this subtree is now a separate component
        }

        // Return sum for parent's calculation
        return sum;
    }

    public static void main(String[] args) {
        MaximumNumberOfKDivisibleComponents solution = new MaximumNumberOfKDivisibleComponents();
        // Test Case 1: Example from the problem
        System.out.println("Test Case 1:");
        int n1 = 5;
        int[][] edges1 = {{0,2}, {1,2}, {1,3}, {2,4}};
        int[] values1 = {1,8,1,4,4};
        int k1 = 6;
        System.out.println("Expected: 2");
        System.out.println("Result: " + solution.maxKDivisibleComponents(n1, edges1, values1, k1));
    }
}

```





