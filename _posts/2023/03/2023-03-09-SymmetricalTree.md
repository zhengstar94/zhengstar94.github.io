---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Symmetrical Tree"
date: "2023-03-09"
categories:
  - "Algorithm Binary Trees"
---

# Symmetrical Tree [Medium]

- Write a function that takes in a Binary Tree and returns if that tree is symmetrical. A tree is symmetrical if the left and right subtrees are mirror images of each other.

- Each BinaryTree node has an integer `value`, a `left` child node, and a `right` child node. Children nodes can either be 1BinaryTree1 nodes themselves or `None`/`null`.



**Sample Input **

````
tree1 =               1
                   /     \
                 2         2
               /   \     /   \
             3      4   3     4
           /   \             /  \
         5       6         6      5
````

**Sample Output **

```
True
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> It's important to first think about what it means for a binary tree to be symmetrical. The left and right subtrees do not need to be the same, but rather they need to be mirror images of each other (i.e. the same if one is inverted).</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> It can be helpful to think about this problem one step at a time. Looking at just the first node, how can you ensure its children are symmetrical? Then looking at those children, how can you make sure they are symmetrical of each other?</strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> This problem can be solved either recursively or iteratively. Either way, try traversing through the tree, uses a mirrored traversal on one side, and check that the values of each node are the same. </strong></i>
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
 * @date 2023/02/26
 */
public class SymmetricalTree {

    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

    public boolean symmetricalTree(BinaryTree root) {
        // The main function to check if a binary tree is symmetrical
        // It takes the root node of the binary tree as input and returns a boolean value
        return isMirror(root, root);
    }

    private boolean isMirror(BinaryTree root1, BinaryTree root2) {
        // A helper function to recursively check if two subtrees are mirrors of each other
        // It takes two nodes as input and returns a boolean value
        // If both nodes are null, they are considered as mirrors of each other
        if (null == root1 && null == root2) {
            return true;
        }
        // If one of the nodes is null, but the other is not, they are not mirrors of each other
        if(null == root1 || null == root2){
            return false;
        }

        // Check if the values of the two nodes are equal
        // If they are, recursively check if the left subtree of the first node is a mirror of the right subtree of the second node
        // and if the right subtree of the first node is a mirror of the left subtree of the second node
        // If both checks return true, the two nodes are mirrors of each other
        return root1.value == root2.value
                && isMirror(root1.right, root2.left)
                && isMirror(root1.left, root2.right);
    }
}

```



