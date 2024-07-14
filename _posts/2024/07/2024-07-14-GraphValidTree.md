---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "261.Graph Valid Tree"
date: "2024-07-14"
categories:
  - "LeetCode Graphs"
---


# 261. Graph Valid Tree

- Given `n` nodes labeled from `0` to `n-1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

**Example 1**

```
Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]
Output: true
```

**Example 2**

```
Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
Output: false
```

## Method 1

```tex
【O(n + E) time | O(n) space】
```

```java
package Leetcode.Graphs;

/**
 * A class to determine if a given graph is a valid tree.
 * @author zhengstars
 * @date 2024/07/14
 */
public class GraphValidTree {

    /**
     * Determines if a graph with 'n' nodes and 'edges' is a valid tree.
     * @param n Number of nodes in the graph.
     * @param edges Edges representing connections between nodes.
     * @return True if the graph is a valid tree, false otherwise.
     */
    public static boolean validTree(int n, int[][] edges) {
        // Initialize an array to keep track of parent nodes for each node
        int[] parent = new int[n];

        // Initialize each node's parent to itself (initially each node is its own parent)
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }

        // Iterate through each edge in the graph
        for (int[] edge : edges) {
            // Find the root parent of both nodes of the current edge
            int u = findParent(parent, edge[0]);
            int v = findParent(parent, edge[1]);

            // If both nodes have the same root parent, a cycle is detected
            if (u == v) {
                return false; // It's not a valid tree if there's a cycle
            }

            // Union operation: Set the parent of v's root to u's root to merge the sets
            parent[v] = u;
        }

        // Count the number of distinct root parents
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (parent[i] == i) {
                count++;
                // If more than one distinct root parent is found, the graph is not fully connected
                if (count > 1) {
                    return false; // It's not a valid tree if it's not fully connected
                }
            }
        }

        // If there is exactly one distinct root parent, the graph is a valid tree
        return count == 1;
    }

    /**
     * Finds the root parent of a node using path compression technique.
     * @param parent Array representing parent nodes for each node.
     * @param node The node whose root parent needs to be found.
     * @return The root parent of the node.
     */
    private static int findParent(int[] parent, int node) {
        while (parent[node] != node) {
            node = parent[node]; // Path compression: set current node's parent to its grandparent
        }
        return node; // Return the root parent of the node
    }

    /**
     * Main method to test the GraphValidTree class.
     * @param args Command line arguments (not used).
     */
    public static void main(String[] args) {
        // Test case 1
        int n1 = 5;
        int[][] edges1 = { {0,1}, {0,2}, {0,3}, {1,4}};
        System.out.println(validTree(n1, edges1)); // Expected output: true

        // Test case 2
        int n2 = 5;
        int[][] edges2 = { {0,1}, {1,2}, {2,3}, {1,3}, {1,4}};
        System.out.println(validTree(n2, edges2)); // Expected output: false
    }
}

```

