---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Find Nodes Distance K"
date: "2023-03-12"
categories:
  - "Algorithm Binary Trees"
---

# Find Nodes Distance K [Hard]

- You're given the root node of a Binary Tree, a `target` value of a node that's contained in the tree, and a positive integer `k`. Write a function that returns the values of all the nodes that are exactly distance `k` from the node with `target` value.

- The distance between two nodes is defined as the number of edges that must be traversed to go from one node to the other. For example, the distance between a node and its immediate left or right child is `1`. The same holds in reverse:the distance between a node and its parent is `1`. In a tree of three nodes where the root node has a left and right child, the left and right children are distance `2` from each other.

- Each `BinaryTree` node has an integer `value`, a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/`null`.

- Note that all `BinaryTree` node values will be unique, and your function can return the output values in any order.





**Sample Input **

````
tree1 =               1
                   /     \
                 2         3
               /   \         \
             4      5         6
                            /   \
                           7     8
                           
target = 3
k = 2
````

**Sample Output **

```
[2,7,8] //These values could be ordered differently.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Would it be easier to solve this problem if you had information about every node's parent node?</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> One approach to this problem is to find the parent nodes of all nodes in the tree. With this information you can perform a breadth-first search starting at the target node and traverse through each neighbor (left, right, and parent node)of every node, keeping track of your distance from the target node at each iteration. Once you reach a node that is distance k from the target node,you can add it to your output array. You'll have to also keep track of which nodes you've visited so as to avoid visiting the same nodes over and over again. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Another approach is to use a recursive depth-first-search algorithm as follows:<br>
      <br>
      - Case #1: when currentNode = target search the subtree rooted at currentNode for all nodes that are k distance from currentNode.<br>
      - Case #2: when target is in the left subtree of currentNode at distance L + 1, look for nodes that are distance k - L - 1 in the right subtree of currentNode.<br>
      - Case #3: when target is in the right subtree of currentNode at distance L + 1, do the same thing as in case #2 but in the opposite subtree.<br>
      - Case #4: when target is neither in the left nor in right subtree of currentNode, stop recursing.<br>
      </strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package BT;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/02/28
 */
public class FindNodesDistanceK2 {
    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

    public static List<Integer> distanceK(BinaryTree root, int target, int k) {
        ArrayList<Integer> res = new ArrayList<>();

        // If the distance is 0, the target node is the only node at that distance from itself
        if (k == 0) {
            res.add(target);
            return res;
        }

        // Create a map of nodes and their neighbors in the binary tree
        Map<BinaryTree, ArrayList<BinaryTree>> map = new HashMap<>();
        preOrder(map, root, null);

        // Keep track of visited nodes to avoid visiting them again
        Set<BinaryTree> visited = new HashSet<>();

        // Use a queue for a breadth-first search to find nodes at distance k from the target
        Deque<BinaryTree> queue = new ArrayDeque<>();

        // Find the target node in the binary tree
        BinaryTree targetNode = null;
        for (BinaryTree node : map.keySet()) {
            if (node.value == target) {
                targetNode = node;
                break;
            }
        }

        // If the target node isn't found, return an empty list
        if (targetNode == null) {
            return res;
        }

        // Start the search with the target node and add it to the queue and visited set
        queue.offer(targetNode);
        visited.add(targetNode);
        int dist = 0;

        // Continue the search while the queue is not empty and the distance is less than or equal to k
        while (!queue.isEmpty()) {
            int size = queue.size();
            dist++;

            // Process each node in the queue's level
            for (int i = 0; i < size; i++) {
                BinaryTree cur = queue.poll();
                ArrayList<BinaryTree> neighbors = map.get(cur);

                // Check each neighbor of the current node
                for (BinaryTree neighbor : neighbors) {
                    if (!visited.contains(neighbor)) {
                        visited.add(neighbor);
                        queue.offer(neighbor);

                        // If the neighbor is at distance k, add its value to the result list
                        if (dist == k) {
                            res.add(neighbor.value);
                        }
                    }
                }
            }

            // If the distance is k, we have found all nodes at distance k and can exit early
            if (dist == k) {
                break;
            }
        }

        return res;
    }


    private static void preOrder(Map<BinaryTree, ArrayList<BinaryTree>> map, BinaryTree node, BinaryTree parent) {
        if (node == null) {
            return;
        }
        // If the parent node is not null, add the current node as a child of the parent node in the map.
        if (parent != null) {
            map.computeIfAbsent(node, k -> new ArrayList<>()).add(parent);
            // Add the parent node as a child of the current node in the map.
            map.computeIfAbsent(parent, k -> new ArrayList<>()).add(node);
        }
        // Recursively traverse the left subtree.
        preOrder(map, node.left, node);
        // Recursively traverse the right subtree.
        preOrder(map, node.right, node);
    }

    public static void main(String[] args) {
        BinaryTree binaryTree = new BinaryTree(1);
        distanceK(binaryTree,1,1);
    }

}

```



