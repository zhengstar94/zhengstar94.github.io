---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Validate Three Nodes"
date: "2023-02-09"
categories:
  - "Algorithm BST"
---


# Validate Three Nodes [Hard]

- You're given three nodes that are contained in the same Binary Search Tree:`nodeone`, `nodeTwo` and `nodeThree` Write a function that returns a boolean representing whether one of `nodeOne` or `nodeThree` is an ancestor of `nodeTwo` and the other node is a descendant of nodeTwo For example,if your function determines that `nodeone` is an ancestor of `nodeTwo` then it needs to see if `nodeThree` is a descendant of `nodeTwo` If your function determines that `nodeThree` is an ancestor,then it needs to see if `nodeOne` is a descendant.

- A descendant of a node `N` is defined as a node contained in the tree rooted at `N` A node `N` is an ancestor of another node `M` if `M` is a descendant of `N`.

- It isn't guaranteed that `nodeOne` or `nodeThree` will be ancestors or descendants of `nodeTwo` but it is guaranteed that all three nodes will be unique and will never be `None`/ `null` In other words, you'll be given valid input nodes.

- Each `BST` node has an integer `value` a `left` child node,and a `right` child node.A node is said to be a valid `BST` node if and only if it satisfies the BST property:its `value` is strictly greater than the values of every node to its left;its `value` is less than or equal to the values of every node to its right;and its children nodes are either valid `BST` nodes themselves or `None`/`null`





**Sample Input **


```
tree =    5
       /     \
      2       7
    /   \   /   \
   1     4 6     8
  /     /
 0     3
         

//This tree won't actually be passed as an input; it's here to help you visualize the problem
nodeOne 5 //The actual node with value 5. 
nodeTwo 2 //The actual node with value 2. 
nodeThree 3 //The actual node with value 3.
```


**Sample Output **

```
true //nodeOne is an ancestor of nodeTwo,and nodeThree is a descendant of nodeTwo.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Keep in mind that the nodes passed to you are contained in a Binary Search Tree-not just a normal Binary Tree.How might this help you traverse the tree faster? </strong></i>
</details>


<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> There are multiple ways to solve this problem,but the simplest is to just check the possible relationships between the nodes.Since you're looking for a descendant and an ancestor, simply check if nodeone is a descendant of nodeTwo and if it is,then check if
nodeThree is an ancestor of nodeTwo If the previous checks come out negative,check if nodeThree is a descendant of nodeTwo and if it is,then check if nodeOne is an ancestor of nodeTwo.  </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Although the approach mentioned in Hint #2 is fairly efficient (it runs in 0(h) time,where h is the height of the tree),there's a way to solve this problem faster.It involves realizing that, when searching for nodeTwo from either nodeOne or nodeThree if you ever reach nodeThree from nodeOne or nodeOne from nodeThree before reaching nodeTwo then you can immediately stop the algorithm,because nodeTwo cannot be between these nodes.See the Conceptual Overview section of this question's video explanation for a more in-depth explanation.  </strong></i>
</details>

<br>



## Method 1

```tex
【O(n)time∣O(h)space】
```

```java
package BST;

/**
 * @author zhengstars
 * @date 2023/01/20
 */
public class ValidateThreeNodes {
    public static class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        TreeNode() {}
        TreeNode(int val) { this.val = val; }
        TreeNode(int val, TreeNode left, TreeNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }
    }

    public static boolean validateThreeNodes(TreeNode nodeOne, TreeNode nodeTwo, TreeNode nodeThree) {
        // Determine whether nodeOne is a descendant of nodeTwo
        if(isDescendant(nodeTwo,nodeOne)){
            // Determine whether nodeTwo is a descendant of nodeThree
            return isDescendant(nodeThree,nodeTwo);
        }

        // Determine whether nodeThree is a descendant of nodeTwo
        if(isDescendant(nodeTwo,nodeThree)){
            // Determine whether nodeTwo is a descendant of nodeOne
            return isDescendant(nodeOne,nodeTwo);
        }

        // the relationship among nodeOne,nodeTwo,nodeThree does not meet the requirements. Return false
        return false;
    }

    private static boolean isDescendant(TreeNode node, TreeNode target) {
        // If the node is empty, it is not a child node
        if(null == node){
            return false;
        }

        // Judge whether the nodes are equal
        if(node == target){
            return true;
        }

        // Recursively traverse the left and right nodes and judge with target
        return isDescendant(node.left,target) || isDescendant(node.right,target);
    }

}

```

## Method 2

```tex
【O(n)time∣O(1)space】
```

```java
package BST;

/**
 * @author zhengstars
 * @date 2023/01/20
 */
public class ValidateThreeNodes {
    public static class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        TreeNode() {}
        TreeNode(int val) { this.val = val; }
        TreeNode(int val, TreeNode left, TreeNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }
    }

  

    public static boolean validateThreeNodes(TreeNode nodeOne, TreeNode nodeTwo, TreeNode nodeThree) {
        // Determine whether nodeOne is a descendant of nodeTwo
        if(isDescendant(nodeTwo,nodeOne)){
            // Determine whether nodeTwo is a descendant of nodeThree
            return isDescendant(nodeThree,nodeTwo);
        }

        // Determine whether nodeThree is a descendant of nodeTwo
        if(isDescendant(nodeTwo,nodeThree)){
            // Determine whether nodeTwo is a descendant of nodeOne
            return isDescendant(nodeOne,nodeTwo);
        }

        // the relationship among nodeOne,nodeTwo,nodeThree does not meet the requirements. Return false
        return false;
    }

    private static boolean isDescendant(TreeNode node, TreeNode target) {
        // Loop through the node, node is not empty and node is not equal to target
        while(node != null && node != target){
            // If the current node value is greater than the target node value, traverse the left subtree
            if(target.val < node.val){
                node = node.left;
            }else{
                // If the current node value is less than the target node value, traverse the right subtree
                node = node.right;
            }
        }
        // To judge whether the node after traversal is the target node, yes means it is the descendant of the target node, otherwise it is not.
        return node == target;
    }
}

```



## Method 3

```tex
【O(d)time∣O(1)space】
```

```java
package BST;

/**
 * @author zhengstars
 * @date 2023/01/20
 */
public class ValidateThreeNodes {
    public static class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        TreeNode() {}
        TreeNode(int val) { this.val = val; }
        TreeNode(int val, TreeNode left, TreeNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }
    }


    public static boolean validateThreeNodes3(TreeNode nodeOne, TreeNode nodeTwo, TreeNode nodeThree) {
         // Check if nodeOne is ancestor of nodeTwo
         boolean isNodeOneAncestor = checkIfAncestor(nodeOne,nodeTwo);
         // Check if nodeThree is ancestor of nodeTwo
         boolean isNodeThreeAncestor =  checkIfAncestor(nodeThree,nodeTwo);
         // Check if nodeOne is descendant of nodeTwo
         boolean isNodeOneDescendant = checkIfAncestor(nodeTwo,nodeOne);
         // Check if nodeThree is descendant of nodeTwo
         boolean isNodeThreeDescendant =  checkIfAncestor(nodeTwo,nodeThree);

         // return true if either nodeOne is ancestor and nodeThree is descendant of nodeTwo
         // or nodeThree is ancestor and nodeOne is descendant of nodeTwo
         return (isNodeOneAncestor && isNodeThreeDescendant) || (isNodeThreeAncestor && isNodeOneDescendant);
    }

    private static boolean checkIfAncestor(TreeNode ancestor,TreeNode descendant) {
        // Initialize currentNode with ancestor
        TreeNode currentNode = ancestor;
        // Iterate through the tree
        while (currentNode != null) {
            // if descendant is equal to currentNode then return true
            if(descendant.val == currentNode.val){
                return true;
            }else if(descendant.val < currentNode.val){
                // if descendant value is less than currentNode value then go to left subtree
                currentNode = currentNode.left;
            }else{
                // if descendant value is greater than currentNode value then go to right subtree
                currentNode = currentNode.right;
            }
        }
        return false;
    }

}

```





