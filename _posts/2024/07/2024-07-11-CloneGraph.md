---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "133.Clone Graph"
date: "2024-07-11"
categories:
  - "LeetCode Graphs"
---

# 133. Clone Graph

- Given a reference of a node in a **[connected](https://en.wikipedia.org/wiki/Connectivity_(graph_theory)#Connected_graph)** undirected graph.
- Return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) (clone) of the graph.
- Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

> ```
> class Node {
>     public int val;
>     public List<Node> neighbors;
> }
> ```

**Test case format:**

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with `val == 1`, the second node with `val == 2`, and so on. The graph is represented in the test case using an adjacency list.

**An adjacency list** is a collection of unordered **lists** used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the **copy of the given node** as a reference to the cloned graph.

**Example 1**

```
Input: adjList = [ [2,4],[1,3],[2,4],[1,3]]
Output: [ [2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

**Example 2**

```
Input: adjList = [ []]
Output: [ []]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
```

**Example 3**

```
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Graphs;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * This class contains a solution for the LeetCode problem "133. Clone Graph".
 * Given a reference of a node in a connected undirected graph, clone the graph and return the reference of the cloned node.
 *
 * @author zhengstars
 * @date 2024/07/11
 */
public class CloneGraph {

  /**
   * Clone the given graph starting from the given node.
   * @param node the starting node to clone
   * @return the reference of the cloned node
   */
  public Node cloneGraph(Node node) {
    // Handle the case where the input node is null
    if (node == null) {
      return null;
    }

    // Create a mapping to store the original nodes and their corresponding cloned nodes
    Map<Node, Node> map = new HashMap<>();
    return cloneGraphDFS(node, map);
  }

  /**
   * Depth-first search function to recursively clone the graph.
   * @param node the current node to clone
   * @param map a mapping of original nodes to cloned nodes
   * @return the cloned node
   */
  private Node cloneGraphDFS(Node node, Map<Node, Node> map) {
    // If the node is already cloned, return the cloned node
    if (map.containsKey(node)) {
      return map.get(node);
    }

    // Create a new node with the same value as the original node
    Node newNode = new Node(node.val);
    map.put(node, newNode);

    // Recursively clone the neighbors of the node
    for (Node neighbor : node.neighbors) {
      newNode.neighbors.add(cloneGraphDFS(neighbor, map));
    }

    return newNode;
  }

  /**
   * Main method to test the clone graph functionality.
   */
  public static void main(String[] args) {
    CloneGraph cg = new CloneGraph();

    // Create a test case with nodes and neighbors
    Node node1 = new Node(1);
    Node node2 = new Node(2);
    Node node3 = new Node(3);
    Node node4 = new Node(4);

    // Establish neighbor relationships
    node1.neighbors.add(node2);
    node1.neighbors.add(node4);
    node2.neighbors.add(node1);
    node2.neighbors.add(node3);
    node3.neighbors.add(node2);
    node3.neighbors.add(node4);
    node4.neighbors.add(node1);
    node4.neighbors.add(node3);

    // Clone the graph starting from node1
    Node clone = cg.cloneGraph(node1);

    // Print whether the original and clone have the same structure
    System.out.println("Original and Clone have the same structure: " + (clone.val == node1.val));
  }
}

/**
 * Node class representing a single node in the graph.
 */
class Node {
  public int val;
  public List<Node> neighbors;

  /**
   * Constructor to create a new Node with a given value and an empty list of neighbors.
   * @param val the value of the node
   */
  public Node(int val) {
    this.val = val;
    this.neighbors = new ArrayList<>();
  }
}

```

