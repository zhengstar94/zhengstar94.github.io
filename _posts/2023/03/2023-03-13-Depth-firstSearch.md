---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Depth-first Search"
date: "2023-03-13"
categories:
  - "Algorithm Graphs"
---

# Depth-first Search [Easy]

- You're given a `Node` class that has a `name` and an array of optional `children` nodes. When put together, nodes form an acyclic tree-like structure.

- Implement the `depthFirstSearch` method on the `Node` class, which takes in an empty array,traverses the tree using the Depth-first Search approach (specifically navigating the tree from left to right), stores all of the nodes'names in the input array,and returns it.

- If you're unfamiliar with Depth-first Search, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.



**Sample Input **

````
tree1 =               A
                   /  |  \
                 B    C    D
                / \       /  \
               E    F    G    H
                   / \     \
                  I   J     K

````

**Sample Output **

```
["A","B","E","F","I","J","C","D","G","K","H"]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> The Depth-first Search algorithm works by traversing a graph branch by branch. In other words, before traversing any Node's sibling Nodes, its children nodes must be traversed. How can you simply and effectively keep track of Nodes'sibling Nodes as you traverse them, all the while retaining the order in which you must traverse them?</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Start at the root Node and try simply calling the depthFirstSearch method on all of its children Nodes. Then, call the depthFirstSearch method on all children Nodes of each child node. Keep applying this logic until the entire graph has been traversed. Don't forget to add the current Node's name to the input array at every call of depthFirstSearch. </strong></i>
</details>

<br>

## Method 1

```tex
【 O(v+e)time | O(e) space 】\\
 \\
```

<br>


```tex
1.\ V\ is\ the\ number\ of\ nodes\ in\ the\ tree\\
2.\ E\ is\ the\ number\ of\ edges\ in\ the\ tree
```



```java
package Graphs;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/03/04
 */
public class DepthfirstSearch {

    static class Node {
        String name;
        List<Node> children = new ArrayList<Node>();

        public Node(String name) {
            this.name = name;
        }

        // Depth-first search method, traversing all nodes and storing node names in the input array
        public List<String> depthFirstSearch(List<String> array) {
            array.add(this.name); // Adds the current node name to the input array
            for (Node child : this.children) { // Traverses all the children of the current node
                child.depthFirstSearch(array); // Recursively traverse child nodes
            }
            return array; // The input array after traversing all the nodes
        }

        public Node addChild(String name) {
            Node child = new Node(name);
            children.add(child);
            return this;
        }
    }
}

```