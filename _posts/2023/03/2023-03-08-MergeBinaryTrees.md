---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Merge Binary Trees"
date: "2023-03-08"
categories:
  - "Algorithm Binary Trees"
---

# Merge Binary Trees [Medium]

- Given two binary trees, merge them and return the resulting tree. If two nodes overlap during the merger then sum the values, otherwise use the existing node.

- Note that your solution can either mutate the existing trees or return a new tree.





**Sample Input **

````
tree1 =      1
          /    \
         3       2
       /   \    
      7     4

tree2 =      1
          /    \
         5       9
       /       /   \    
     2        7     4
````

**Sample Output **

```
output =     2
          /    \
         8      11
       /  \    /   \    
     2     4  7     6
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> lf the function takes two tree nodes as parameters then what should be returned if either of the two nodes is null? Remember,if two nodes overlap during the merger then sum the values, otherwise use the existing node.How can you sum the tree node values when they overlap?</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> If two tree nodes overlap then sum the values into either one of the nodes. This node will be returned from the function. Recursively call the function twice passing in both trees'left nodes as well as their right nodes..</strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> The iterative approach to this problem uses a stack in replacement of the recusions stack space. What would you push onto the stack in order to traverse and merge the binary trees? </strong></i>
</details>

<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> You can either use a single stack and push associated pairs of nodes on the stack, or you can maintain a stack for each tree. </strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/25
 */
public class MergeBinaryTrees {

    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

    public BinaryTree mergeBinaryTrees(BinaryTree tree1, BinaryTree tree2) {
        // If tree1 is null, then the merged tree is just tree2.
        if (null == tree1) {
            return tree2;
        }

        // If tree2 is null, then the merged tree is just tree1.
        if (null == tree2) {
            return tree1;
        }

        // Create a new binary tree node with the sum of values of tree1 and tree2.
        BinaryTree tree = new BinaryTree(tree1.value + tree2.value);

        // Recursively merge the left subtrees of tree1 and tree2 and set it to the left child of the new node.
        tree.left = mergeBinaryTrees(tree1.left, tree2.left);

        // Recursively merge the right subtrees of tree1 and tree2 and set it to the right child of the new node.
        tree.right = mergeBinaryTrees(tree1.right, tree2.right);

        // Return the root of the merged binary tree.
        return tree;
    }
}

```



## Method 2

```tex
【O(n)time∣O(1)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/25
 */
public class MergeBinaryTrees {

    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

    
    public BinaryTree mergeBinaryTrees1(BinaryTree tree1, BinaryTree tree2) {
        // If tree1 is null, return tree2 since there is nothing to merge
        if(null == tree1){
            return tree2;
        }

        // If tree2 is null, return tree1 since there is nothing to merge
        if(null == tree2){
            return tree1;
        }

        // Update the value of tree1 by adding the value of tree2
        tree1.value += tree2.value;

        // Merge the left subtrees of tree1 and tree2
        tree1.left = mergeBinaryTrees(tree1.left,tree2.left);

        // Merge the right subtrees of tree1 and tree2
        tree1.right = mergeBinaryTrees(tree1.right,tree2.right);

        // Return the merged tree, which is tree1
        return tree1;
    }
}

```





