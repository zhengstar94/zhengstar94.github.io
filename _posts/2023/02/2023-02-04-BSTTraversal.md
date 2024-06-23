---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "BST Traversal"
date: "2023-02-04"
categories:
  - "Algorithm BST"
---

# BST Traversal [Medium]

- Write three functions (in-order, pre-order, and post-order traversal) that takes in a Binary Search Tree (BST) and an empty array, traverse the BST add the node's value to the input array and return the array.

**Sample Input**

```\
tree =              10
	              /     \
		        5	      15
	          /   \          \
		     2     6          22
	       /
		 1
```

**Sample Output**

```
in_order = [1, 2, 5, 6, 10, 15, 22]
pre_order = [10, 5, 2, 1, 6, 15, 22]
post_order = [1, 2, 6s, 5, 22, 15, 10]
```

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Realize that in-order traversal simply means traversing left nodes before traversing current nodes before traversing right nodes.Try implementing this algorithm recursively by calling the inOrderTraverse method on a left node,then appending the current node's value to the input array,and then calling the inOrderTraverse method on a right node. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Apply the same logic described in Hint #1 for the two other traversal methods,but change the order in which you do things.  </strong></i>
</details>

<br>


## Method 1(Recursive)

```tex
【 O(n)time | O(h) space 】
```

```java
package NewArray;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/01/06
 */
public class BSTTraversal {

    public class TreeNode {
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

    /**
     * Mid-Order traversal
     * left mid right
     * @param root
     * @return
     */
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        inOrder(list,root);
        return list;
    }

    public static void inOrder(List<Integer> list, TreeNode root ){
        if(null == root){
            return;
        }
        inOrder(list,root.left);
        list.add(root.val);
        inOrder(list,root.right);
    }

    /**
     * Pre-Order traversal
     * mid left right
     * @param root
     * @return
     */
    public List<Integer> preTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        preOrder(list,root);
        return list;
    }

    public static void preOrder(List<Integer> list, TreeNode root ){
        if(null == root){
            return;
        }
        list.add(root.val);
        preOrder(list,root.left);
        preOrder(list,root.right);
    }



    /**
     * Post-order traversal
     * left right mid
     * @param root
     * @return
     */
    public List<Integer> postTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        postOrder(list,root);
        return list;
    }

    public static void postOrder(List<Integer> list, TreeNode root ){
        if(null == root){
            return;
        }
        postOrder(list,root.left);
        postOrder(list,root.right);
        list.add(root.val);
    }
}

```



## Method 2(Iteration)

```tex
【 O(n)time | O(h) space 】
```

```java
package NewArray;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/01/06
 */
class TreeNodeTraversal2 {
    static class TreeNode {
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

    public static List<Integer> inorderTraversal(TreeNode root) {
        // Stack first in and then out
        // Traversal in the middle order, out-of-stack order: left root right; stack order: right root left
        //中序遍历，出栈顺序：左根右; 入栈顺序：右根左
        List<Integer> list = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();

        // Root is empty and stack is empty. Traversal ends.
        while(null != root || !stack.isEmpty()){
            // First root and then left into the stack
            while(null != root){
                stack.push(root);
                root = root.left;
            }

            // Root==null at this time indicates that the root in the previous step does not have a left subtree.
            // 1. Execute the left exit stack. Because root==null at this time, root.right must be null
            // 2. Execute the next outer while code block and root it out of the stack. Root.right may exist at this time
            // 3a. If root.right exists, enter the stack right and then exit the stack.
            // 3b. If root.right does not exist, repeat step 2
            root = stack.pop();
            list.add(root.val);
            root = root.right;
        }

        return list;
    }

    public static List<Integer> preorderTraversal(TreeNode root) {

        List<Integer> list = new ArrayList<Integer>();
        // Create a stack to store the nodes that will be traversed
        Stack<TreeNode> stack = new Stack<>();
        // If the root node is not empty, press the root node into the stack
        if (root != null) {
            stack.push(root);
        }

        // Execute a loop when the stack is not empty
        while (!stack.empty()) {
            // Remove the elements at the top of the stack
            TreeNode node = stack.pop();

            list.add(node.val);


            // **Because the order of entering the stack is the right and left root, **
            // **you need to judge the right to enter the stack before judging the left**

            // If the current node has a right child node, press the right child node into the stack
            if (node.right != null) {
                stack.push(node.right);
            }
            // If the current node has a left child node, press the left child node into the stack
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        return list;
    }

    public static List<Integer> postorderTraversal(TreeNode root) {

        List<Integer> list = new ArrayList<Integer>();

        if(root == null){
            return list;
        }

        // Create a stack to store the nodes that will be traversed
        Stack<TreeNode> stack = new Stack<>();

        // Variable is used to record whether the right subtree has been accessed
        TreeNode preNode = null;

        //Main ideas:
        //Because after the access to a child tree is completed, it has to be traced back to its parent node

        //Therefore, prev can be used to record the access history,
        // which can be used to determine whether the last accessed node is a right subtree when backtracking to the parent node.
        while(null != root || !stack.isEmpty()){
            while(null != root){
                stack.push(root);
                root = root.left;
            }
            //For the elements popping up from the stack, the left subtree must have been accessed.
            root = stack.pop();

            //What we need to determine now is whether there is a right subtree, or whether the right subtree has been visited.
            //If there is no right subtree, or if the right subtree is finished, that is, when the last node visited is the right child node
            //Indicates that the current node can be accessed
            if(null == root.right || root.right == preNode){
                list.add(root.val);
                //Update the historical access record so that when backtracking, the parent node can determine whether the access to the right child tree is complete.
                preNode = root;
                root = null;
            }else{
                //If the right subtree is not accessed, press the current node on the stack and access the right subtree
                stack.push(root);
                root = root.right;
            }
        }

        return list;
    }


    public static void main(String[] args) {
        TreeNode tree = new TreeNode(10);

        TreeNode tree1 = new TreeNode(5);
        tree1.left = new TreeNode(2);
        tree1.right = new TreeNode(6);

        TreeNode tree2 = new TreeNode(15);
        tree2.left = new TreeNode(12);
        tree2.right = new TreeNode(22);

        tree.left = tree1;
        tree.right = tree2;

        System.out.println(preorderTraversal(tree));
    }
}
```


## Method 3(Morris)

```tex
【 O(n)time | O(1) space 】
```

```java
package NewArray;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/01/09
 */
public class BSTTraversal4 {

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


    /**
     * Mid-order traversal
     * You can only come to your own node once and print directly.
     * The node that can come to you twice will only print the second time.
     * @param root
     * @return
     */
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if(null == root){
            return list;
        }

        TreeNode cur = root;

        TreeNode morrisRight = null;

        // Cur is empty to stop traversing
        while(null != cur){
            //morrisRight is cur's left child.
            morrisRight = cur.left;

            // There is a left subtree
            if(null != morrisRight){

                // After while finished running,
                // morrisRight came to the position of the rightmost child in the left subtree of cur
                // Find the rightmost node of the left subtree
                while(null != morrisRight.right && morrisRight.right != cur){
                    morrisRight = morrisRight.right;
                }

                // Come to the location of cur for the first time
                if(null == morrisRight.right){
                    morrisRight.right = cur;
                    cur = cur.left;
                    continue;// The first time you come in cur, just start the continue,while loop again.
                }else{//When came to cur,morrisRight for the second time, the right pointer was broken.
                    //Enter cur for the second time and execute it directly.
                    morrisRight.right = null;
                }
            }
            // You can only come to your own node once and print directly.
            // The node that can come to you twice will only print the second time.
            list.add(cur.val);
            //If the left child is empty, Then directly access the right subtree of the node
            cur = cur.right;
        }
        return list;
    }

    /**
     * The node print that appears for the first time, that is
     * a.There is no direct printing of the left node, and the description of the left node is printed twice.
     * b.Come to cur printing for the first time
     * @param root
     * @return
     */
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if(null == root){
            return list;
        }

        TreeNode cur = root;

        TreeNode morrisRight = null;

        // Cur is empty to stop traversing
        while(null != cur){
            //morrisRight is cur's left child.
            morrisRight = cur.left;

            // There is a left subtree
            if(null != morrisRight){

                // After while finished running,
                // morrisRight came to the position of the rightmost child in the left subtree of cur
                // Find the rightmost node of the left subtree
                while(null != morrisRight.right && morrisRight.right != cur){
                    morrisRight = morrisRight.right;
                }

                // Come to the location of cur for the first time
                if(null == morrisRight.right){
                    //  Come to the cur node for the first time
                    list.add(cur.val);

                    morrisRight.right = cur;
                    cur = cur.left;
                    continue;// The first time you come in cur, just start the continue,while loop again.
                }else{//When came to cur,morrisRight for the second time, the right pointer was broken.
                    //Enter cur for the second time and execute it directly.
                    morrisRight.right = null;
                }
            }else{
                // Only can come to yourself once.
                list.add(cur.val);
            }
            //If the left child is empty, Then directly access the right subtree of the node
            cur = cur.right;
        }
        return list;
    }


    public static List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if(null == root){
            return list;
        }

        TreeNode cur = root;

        TreeNode morrisRight = null;

        // Cur is empty to stop traversing
        while(null != cur){
            //morrisRight is cur's left child.
            morrisRight = cur.left;

            // There is a left subtree
            if(null != morrisRight){

                // After while finished running,
                // morrisRight came to the position of the rightmost child in the left subtree of cur
                // Find the rightmost node of the left subtree
                while(null != morrisRight.right && morrisRight.right != cur){
                    morrisRight = morrisRight.right;
                }

                // Come to the location of cur for the first time
                if(null == morrisRight.right){
                    morrisRight.right = cur;
                    cur = cur.left;
                    continue;// The first time you come in cur, just start the continue,while loop again.
                }else{//When came to cur,morrisRight for the second time, the right pointer was broken.
                    //Enter cur for the second time and execute it directly.
                    morrisRight.right = null;

                    // Print the right boundary of the left subtree of cur points in reverse order
                    reversePrintEdge(cur.left,list);
                }
            }
            //If the left child is empty, Then directly access the right subtree of the node
            cur = cur.right;
        }
        // Finally, print the right boundary of the whole tree in reverse order.
        reversePrintEdge(root,list);

        return list;
    }

    /**
     * Take head as the head of the tree, reverse print its boundary
     * @param head
     * @param list
     */
    private static void reversePrintEdge(TreeNode head, List<Integer> list) {
        // Get the tail pointer, which is the rightmost point.
        TreeNode tail = reverse(head);
        // Will print from the tail pointer
        TreeNode cur = tail;
        while(cur != null) {
            list.add(cur.val);
            cur = cur.right;
        }
        // Reverse it at this time. Restore the original tree
        reverse(tail);
    }

    /**
     * Similar to the reverse change of the pointer of a single linked list,
     * the reverse of a single linked list
     * @param from
     * @return
     */
    private static TreeNode reverse(TreeNode from) {
        TreeNode pre = null;
        TreeNode next = null;
        while(from != null) {
            // From is not equal to null here, because after conversion to next, the last one is null
            next = from.right;
            from.right = pre;
            pre = from;
            from = next;
        }

        return pre;
    }

    public static void main(String[] args) {
        TreeNode tree = new TreeNode(1);

        TreeNode tree1 = new TreeNode(2);
        TreeNode tree2 = new TreeNode(3);

        TreeNode tree4 = new TreeNode(4);
        tree4.left = new TreeNode(8);
        tree4.right = new TreeNode(9);

        tree1.left = tree4;
        tree1.right = new TreeNode(5);

        TreeNode tree5 = new TreeNode(7);
        tree5.left = new TreeNode(10);
        tree5.right = new TreeNode(11);

        tree2.left = new TreeNode(6);
        tree2.right = tree5;

        tree.left = tree1;
        tree.right = tree2;

        List<Integer> list =  postorderTraversal(tree);

        System.out.println(list);
    }
}

```