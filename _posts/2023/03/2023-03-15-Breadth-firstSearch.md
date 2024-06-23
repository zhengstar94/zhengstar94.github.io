---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Breadth-first Search"
date: "2023-03-15"
categories:
  - "Algorithm Graphs"
---

# Breadth-first Search [Medium]

- You're given a `Node` class that has a `name` and an array of optional `children` nodes. When put together, nodes form an acyclic tree-like structure.

- Implement the `breadthFirstSearch` method on the `Node` class, which takes in an empty array, traverses the tree using the Breadth-first Search approach (specifically navigating the tree from left to right), stores all of the nodes'names in the input array, and returns it.

- If you're unfamiliar with Breadth-first Search, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.





**Sample Input **

````
graph =               A
                   /  |  \
                 B    C    D
                / \       /  \
               E    F    G    H
                   / \     \
                  I   J     K
````

**Sample Output **

```
["A","B","C","D","E","F","G","H","I","J","K"]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> The Breadth-first Search algorithm works by traversing a graph level by level. In other words, before traversing any Node's children Nodes, its sibling nodes must be traversed. How can you simply and effectively keep track of Nodes'children Nodes as you traverse them, all the while retaining the order in which you must traverse them? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Try using a queue to store all of the future Nodes that you will need to explore as your traverse the graph. By adding Nodes'children Nodes to the queue every time you explore them and by using the First-In-First-Out property of the queue, you can traverse the graph in a Breadth-first Search way. Don't forget to add every Node's name to the input array as you traverse the graph. </strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Graphs;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

/**
 * @author zhengstars
 * @date 2023/03/06
 */
public class BreadthFirstSearch {

    static class Node {
        String name;
        List<Node> children = new ArrayList<Node>();

        public Node(String name) {
            this.name = name;
        }

        public List<String> breadthFirstSearch(List<String> array) {
            Queue<Node> queue = new LinkedList<>();
            // start by adding the root node to the queue
            queue.offer(this);

            // loop while the queue is not empty
            while (!queue.isEmpty()) {
                // dequeue a node from the front of the queue
                Node node = queue.poll();
                // add its name to the output array
                array.add(node.name);

                // enqueue all its children to the back of the queue
                for (Node child : node.children) {
                    queue.offer(child);
                }
            }

            return array; // return the output array
        }

        public Node addChild(String name) {
            Node child = new Node(name);
            children.add(child);
            return this;
        }
    }
}

```

