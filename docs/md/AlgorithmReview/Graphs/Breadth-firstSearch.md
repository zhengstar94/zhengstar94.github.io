# Breadth-first Search

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

## 