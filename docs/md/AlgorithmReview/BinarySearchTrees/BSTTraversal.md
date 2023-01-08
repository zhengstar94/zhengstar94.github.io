# BST Traversal

- Write three functions (in-order, pre-order, and post-order traversal) that takes in a Binary Search Tree (BST) and an empty array, traverse the BST add the node's value to the input array and return the array.

**Sample Usage**

```\
tree =              10
	              /     \
		        5	      15
	          /   \          \
		     2     6          22
	       /
		 1
```

**Sample Result**

```java
in_order = [1, 2, 5, 6, 10, 15, 22]
pre_order = [10, 5, 2, 1, 6, 15, 22]
post_order = [1, 2, 6s, 5, 22, 15, 10]
```



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

