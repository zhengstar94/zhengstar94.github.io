---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Branch Sums"
date: "2023-03-01"
categories:
  - "Algorithm Binary Trees"
---

# Branch Sums [Easy]

- Write a function that takes in a Binary Tree and returns a list of its branch sums ordered from leftmost branch sum to rightmost branch sum.

- A branch sum is the sum of all values in a Binary Tree branch.A Binary Tree branch is a path of nodes in a tree that starts at the root node and ends at any leaf node.

- Each `BinaryTree` node has an integer `value`, a `left` child node,and a `right` child node.Children nodes can either be `BinaryTree` nodes themselves or `None`/`null`



**Sample Input **

````
true =     1
        /      \
      2         3
    /    \     /   \
   4      5   6      7
 /   \   /
2     9 10  
````

**Sample Output **

```
[15,16,18,10,11] 
//15==1+2+4+8 
//16==1+2+4+9 
//18==1+2+5+10 
//10==1+3+6 
//11==1+3+7
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try traversing the Binary Tree in a depth-first-search-like fashion. </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Recursively traverse the Binary Tree in a depth-first-search-like fashion,and pass a running sum of the values of every previously-visited node to each node that you're traversing. </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> As you recursively traverse the tree,if you reach a leaf node (a node with no "left" or "right"Binary Tree nodes),add the relevant running sum that you've calculated to a list of sums (which you'll also have to pass to the recursive function).If you reach a node that isn't a leaf node,keep recursively traversing its children nodes,passing the correctly updated running sum to them.</strong></i>
</details>



<br>



## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package BT;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/02/05
 */
public class BranchSums {
    static class BST {
        public int value;
        public BST left;
        public BST right;
        BST() {}
        public BST(int value) {
            this.value = value;
        }
    }

    public static List<Integer> branchSums(BST root) {
        // Define a list to store the sum of branches of a binary tree
        List<Integer> list =  new ArrayList<Integer>();
        // Call the calculateBranchSums function to calculate the sum of branches of the binary tree
        calculateBranchSums(root,0,list);
        return list;
    }

    private static void calculateBranchSums(BST node, int value, List<Integer> list) {
        // If the current node is empty, exit the function
        if(null == node){
            return;
        }

        // Calculate the sum of the current branches
        int newReturnSum = value + node.value;
        if(null == node.left && null == node.right){
            // If the current node is a leaf node, add branches and to the list
            list.add(newReturnSum);
            return;
        }

        // Call the calculateBranchSums function on the left and right child nodes respectively
        calculateBranchSums(node.left,newReturnSum,list);
        calculateBranchSums(node.right,newReturnSum,list);
    }
}
```





