---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "235.Lowest Common Ancestor of a Binary Search Tree"
date: "2024-03-05"
categories:
  - "LeetCode Trees"
---

# LeetCode 235. Lowest Common Ancestor of a Binary Search Tree 

- Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.
- According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1**

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

**Example 2**

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3**

```
Input: root = [2,1], p = 2, q = 1
Output: 2
```

## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengstars
 * @date 2024/04/07
 */
public class LowestCommonAncestorOfABinarySearchTree {
    // Function to find lowest common ancestor 
    public static TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while(root != null){
            // if root value is less than both p and q, then LCA must be in the right subtree.
            if(root.val < p.val && root.val < q.val){
                root = root.right;
            }
            // if root value is greater than both p and q, then LCA must be in the left subtree.
            else if(root.val > p.val && root.val > q.val){
                root = root.left;
            }
            else{
                // if neither of the above conditions hold true, it implies the current node 
                // is the LCA of p and q.
                return root;
            }
        }
        // if the while loop has completed and no return statement has been hit, 
        // it implies that there is no LCA. Hence, return null.
        return null;
    }

    public static void main(String[] args) {
        // creating a binary search tree
        TreeNode root = new TreeNode(6);
        root.left = new TreeNode(2);
        root.right = new TreeNode(8);
        root.left.left = new TreeNode(0);
        root.left.right = new TreeNode(4);
        root.left.right.left = new TreeNode(3);
        root.left.right.right = new TreeNode(5);
        root.right.left = new TreeNode(7);
        root.right.right = new TreeNode(9);

        // Case 1: Common ancestor for nodes with values 2 and 8
        TreeNode p = root.left; // Node with value 2
        TreeNode q = root.right; // Node with value 8
        // calling LCA function
        TreeNode lca = lowestCommonAncestor(root, p, q);
        System.out.print("Lowest Common Ancestor is: " + lca.val);
        System.out.print("\n");

        // Case 2: Common ancestor for nodes with values 2 and 4
        p = root.left; // Node with value 2
        q = root.left.right; // Node with value 4
        // calling LCA function
        lca = lowestCommonAncestor(root, p, q);
        System.out.print("Lowest Common Ancestor is: " + lca.val);
        System.out.print("\n");

        // Case 3: Common ancestor for nodes with values 2 and 1 in a smaller tree
        root = new TreeNode(2);
        root.left = new TreeNode(1);
        p = root; // Node with value 2
        q = root.left; // Node with value 1
        // calling LCA function
        lca = lowestCommonAncestor(root, p, q);
        System.out.print("Lowest Common Ancestor is: " + lca.val);
        System.out.print("\n");
    }
}
```

