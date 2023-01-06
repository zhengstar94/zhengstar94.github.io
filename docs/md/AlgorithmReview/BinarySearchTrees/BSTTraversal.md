# BST Traversal [Medium]

- Write three functions (in-order, pre-order, and post-order traversal) that takes in a Binary Search Tree (BST) and an empty array, traverse the BST add the node's value to the input array and return the array.

**Sample Usage **

```\
tree =              10
				  /      \
		        5	       15
			  /   \           \
			2       6          22
		  /
		1
```

**Sample Result **

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

