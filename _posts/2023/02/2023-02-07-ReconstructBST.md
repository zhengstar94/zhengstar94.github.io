---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Reconstruct BST"
date: "2023-02-07"
categories:
  - "Algorithm BST"
---

# Reconstruct BST [Medium]

- The pre-order traversal of a Binary Tree is a traversal technique that starts at the tree's root node and visits nodes in the following order:
  1. Current node
  2. Left subtree
  3. Right subtree
- Given a non-empty array of integers representing the pre-order traversal of a Binary Search Tree (BST), write a function that creates the relevant BST and returns its root node.
- The input array will contain the values of BST nodes in the order in which these nodes would be visited with a pre-order traversal.
- Each BST node has an integer value, a left child node, and a right child node. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

**Sample Input **

```
preOrderTraversalValues = [10, 4, 2, 1, 5, 17, 19, 18]
```

**Sample Output **

```
        10 
      /    \
     4      17
   /   \      \
  2     5     19
 /           /
1          18 
```

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Think about the properties of a BST.Looking at the pre-order-traversal nodes (values),how can you determine the right child of a particular node? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The right child of any BST node is simply the first node in the pre-order traversal whose value is larger than or equal to the particular node's value.From this,we know that the nodes in the pre-order traversal that come before the right child of a node must be in the left subtree of that node. </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Once you determine the right child of any given node,you're able to generate the entire left and right subtrees of that node.You can do so by recursively creating the left and right child nodes of each subsequent node using the fact stated in Hint #2.A node that has no left and right children is naturally a leaf node.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> To solve this problem with an optimal time complexity,you need to realize that it's unnecessary to locate the right child of every node.You can simply keep track of the pre- order-traversal position of the current node that needs to be created and try to insert that node as the left or right child of the relevant previously visited node.Since this tree is a BST, every node must satisfy the BST property;by somehow keeping track of lower and upper bounds for node values,you should be able to determine if a node can be inserted as the left or right child of another node.With this approach,you can solve this problem in linear time. See this question's video explanation for a more detailed explanation of this approach.  </strong></i>
</details>

<br>

## Method 1(Brute Force)

```tex
【O(n^2)time∣O(n)space】
```

```java
package BST;

/**
 * @author zhengstars
 * @date 2023/01/15
 */
public class ReconstructBST {

    static class BST {
        public int value;
        public BST left;
        public BST right;
        BST() {}
        public BST(int value) {
            this.value = value;
        }
    }
    
    public static BST constructBST(int[] preorder) {
        if (preorder == null || preorder.length == 0) {
            return null;
        }
        // First create a new root node and assign the first element of the array to the root node
        BST root = new BST(preorder[0]);
        // Iterate through the entire array, starting with index 1
        // and call the insert function insert for each element to insert into the tree
        for (int i = 1; i < preorder.length; i++) {
            insert(root, preorder[i]);
        }
        return root;
    }

    private static void insert(BST root, int i) {
        if(i < root.value){
            if(null == root.left){
                root.left = new BST(i);
            }else{
                insert(root.left,i);
            }
        }else{
            if(null == root.right){
                root.right = new BST(i);
            }else{
                insert(root.right,i);
            }
        }
    }

    public static void main(String[] args) {
        int[] array = new int[]{10,4,2,1,5,17,19,18};
        BST bst = constructBST(array);
        System.out.println(bst);
    }

}

```

## Method 2

```tex
【O(n)time∣O(n)space】
```

```java
package BST;

/**
 * @author zhengstars
 * @date 2023/01/15
 */
public class ReconstructBST {

    static class BST {
        public int value;
        public BST left;
        public BST right;
        BST() {}
        public BST(int value) {
            this.value = value;
        }
    }

    public static BST constructBST(int[] preorder) {
        //Call the recursive function construct to reconstruct the binary search tree

        return construct(preorder, 0, preorder.length - 1, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private static BST construct(int[] preorder, int left, int right, long min, long max) {
        //First determine whether the current index is out of range of the array, and if so, return null.
        if(left > right){
            return null;
        }

        // Determines whether the value of the current node is legal,
        // and returns null if it is illegal (less than or equal to min or greater than or equal to max).
        int val = preorder[left];
        if (val <= min || val >= max) {
            return null;
        }

        //Create the current node
        BST root = new BST(val);
        // and find the first index i in the array that is greater than the current node
        int i = left + 1;
        //The elements before this index will form the left subtree of the current node,
        // and the subsequent elements will form the right subtree of the current node.
        while (i <= right && preorder[i] < val) {
            i++;
        }
        //[10,4,2,1,5,17,19,18]
        // current node is 10
        //left[4,2,1,5]
        //right[17,19,18]


        // 1.1 The construct function is called recursively to reconstruct the left and right subtrees respectively.
        // 1.2 The parameters passed in are the start index and the end index of the left and right subtree,
        // and the value of the current node as a constraint.
        // 1.3 Finally return to the current node.
        root.left = construct(preorder, left + 1, i - 1, min, val);
        root.right = construct(preorder, i, right, val, max);
        return root;
    }

    public static void main(String[] args) {
        int[] array = new int[]{10,4,2,1,5,17,19,18};
        BST bst = constructBST(array);
        System.out.println(bst);
    }

}

```

