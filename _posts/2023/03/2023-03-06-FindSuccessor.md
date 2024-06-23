---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Find Successor"
date: "2023-03-06"
categories:
  - "Algorithm Binary Trees"
---

# Find Successor [Medium]

- Write a function that takes in a Binary Tree (where nodes have an additional pointer to their parent node)as well as a node contained in that tree and returns the given node's successor.

- A node's successor is the next node to be visited (immediately after the given node)when traversing its tree using the in-order tree-traversal technique.A node has no successor if it's the last node to be visited in the in-order traversal.

- If a node has no successor, your function should return `None`/`null`.

- Each `BinaryTree` node has an integer `value`, a `parent` node, a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/`null`.





**Sample Input **

````
true =        1
          /      \
         2         3
       /   \    
      4     5   
    /         
   6		
   
node = 5 
````

**Sample Output **

```
1
// This tree's in-order traversal order is: 
// 6->4->2->5->1->3 
// 1 comes immediately after 5.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Start by performing an in-order traversal of the tree and storing the nodes in an array as you go. Then, traverse the nodes that you've stored;once you find the input node, return the node immediately after it in the array. </strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Can you think of a more time-efficient way to solve this problem without performing the entire in-order traversal?</strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Use the fact that each node has a pointer to its parent to solve this problem in O(h) time, where h is the height of the tree. </strong></i>
</details>


<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> If the given node has a right subtree,then the next node in the in-order traversal is simply the leftmost node in that right subtree. If it doesn't have a right subtree, then we need to traverse up the tree looking for an ancestor of this node that contains the node in question in its left subtree. The first node that we find that contains the input node in its left subtree is the one that will be visited next in the in-order traversal. If we reach the root node, and the input node isn't in the root node's left subtree, then the input node has no successor, because it must be the rightmost node of entire tree. </strong></i>
</details>


<br>

## Method 1

```tex
【O(n)time∣O(h)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/19
 */
public class FindSuccessor2 {

    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;
        public BinaryTree parent = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

    public BinaryTree findSuccessor(BinaryTree tree, BinaryTree node) {
        // If node has a right subtree, successor is the leftmost node in the subtree
        if (node.right != null) {
            BinaryTree currNode = node.right;
            while (currNode.left != null) {
                currNode = currNode.left;
            }
            return currNode;
        }
        // If node has no right subtree, successor is the lowest ancestor whose left child is also an ancestor of the node
        else {
            BinaryTree currNode = node;
            while (currNode.parent != null && currNode == currNode.parent.right) {
                currNode = currNode.parent;
            }
            return currNode.parent;
        }
    }
}

```



## Method 2

```tex
【O(h)time∣O(1)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/19
 */
public class FindSuccessor2 {

    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;
        public BinaryTree parent = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

   
    public static BinaryTree findSuccessor1(BinaryTree tree, BinaryTree node) {
        // if the input node has a right child, its successor must be its leftmost child in the right subtree
        if (node.right != null) {
            return getLeftmostChild(node.right);
        }
        // if the input node has no right child, its successor must be its rightmost parent in the tree
        return getRightmostParent(node);
    }

    // helper function to get the leftmost child of a given node
    public static BinaryTree getLeftmostChild(BinaryTree node) {
        BinaryTree currentNode = node;
        // keep going left until there are no more left children
        while (currentNode.left != null) {
            currentNode = currentNode.left;
        }
        // the current node has no more left children, so it must be the leftmost child
        return currentNode;
    }

    // helper function to get the rightmost parent of a given node
    public static BinaryTree getRightmostParent(BinaryTree node) {
        BinaryTree currentNode = node;
        // keep going up the tree until the current node is a right child or the root node
        while (currentNode.parent != null && currentNode.parent.right == currentNode) {
            currentNode = currentNode.parent;
        }
        // if the current node is a left child, then its parent is the rightmost parent
        return currentNode.parent;
    }

}

```





