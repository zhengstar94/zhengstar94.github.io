# Find Kth Largest Value In BST [Medium]

- Given the root of a binary search tree and an integer k,write a program to return the kth largest value among all the node values in the given BST.
    - If the tree is empty,return null.
    - Assume that all values in the BST nodes are unique.

**Sample Input **

```java
         12
       /     \
      7      17
    /   \   /   \
   3     9 14   
         
         
// If k =2,then 14 is the 2nd largest element.
// If k 4,then 9 is the 4th largest element.
// If k 5,then 7 is the 5th largest element.
```



## Method 1(insert)

```tex
【O(n)time∣O(n)space】
```

```java
package BST;

import NewArray.BstConstruction;
import jdk.nashorn.internal.runtime.regexp.joni.constants.Traverse;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/01/11
 */
public class kthLargestBST {
    static class TreeNode {
        public int value;
        public TreeNode left;
        public TreeNode right;
        TreeNode() {}
        public TreeNode(int value) {
            this.value = value;
        }
    }


    private static int count = 0;
    private static int result = 0;

    public static int kthLargest(TreeNode root, int k) {
        count = k;
        inOrder(root);
        return result;
    }

    private static void inOrder(TreeNode root) {
        if (null == root){
            return;
        }
        inOrder(root.right);
        if(--count == 0){
            result = root.value;
            return;
        }
        inOrder(root.left);
    }

    public static void main(String[] args) {
        TreeNode tree = new TreeNode(12);

        TreeNode tree2 = new TreeNode(7);
        TreeNode tree3 = new TreeNode(17);

        tree2.left = new TreeNode(3);
        tree2.right = new TreeNode(9);

        tree3.left = new TreeNode(14);

        tree.left = tree2;
        tree.right = tree3;

        System.out.println(kthLargest(tree,2));
    }

}

```

## Method 2

```tex
【O(n)time∣O(n)space】
```

```java
package BST;

import NewArray.BstConstruction;
import jdk.nashorn.internal.runtime.regexp.joni.constants.Traverse;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/01/11
 */
public class kthLargestBST {
    static class TreeNode {
        public int value;
        public TreeNode left;
        public TreeNode right;
        TreeNode() {}
        public TreeNode(int value) {
            this.value = value;
        }
    }

    public static int kthLargest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        while(true){
            while (null != root){
                stack.push(root);
                root = root.right;
            }
            root = stack.pop();
            if(--k == 0){
                return root.value;
            }
            root = root.left;
        }

    }

    public static void main(String[] args) {
        TreeNode tree = new TreeNode(12);

        TreeNode tree2 = new TreeNode(7);
        TreeNode tree3 = new TreeNode(17);

        tree2.left = new TreeNode(3);
        tree2.right = new TreeNode(9);

        tree3.left = new TreeNode(14);

        tree.left = tree2;
        tree.right = tree3;

        System.out.println(kthLargest(tree,2));
    }

}

```

## Method 3(Anti-in-order traversal)

```tex
【O(n)time∣O(n)space】
```

```java
package BST;

import NewArray.BstConstruction;
import jdk.nashorn.internal.runtime.regexp.joni.constants.Traverse;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/01/11
 */
public class kthLargestBST {
    static class TreeNode {
        public int value;
        public TreeNode left;
        public TreeNode right;
        TreeNode() {}
        public TreeNode(int value) {
            this.value = value;
        }
    }

    /**
     * Defines a TreeInfo class
     * This class is used to store the values of the node currently traversed and the last node traversed.
     */
    static class TreeInfo {
        public int numberNode;
        public int lastestNode;
        public TreeInfo(int numberNode,int lastestNode) {
            this.numberNode = numberNode;
            this.lastestNode = lastestNode;
        }
    }

    /**
     * Find the k largest element in the binary search tree。
     * It creates a new object of the TreeInfo class and calls the anti-middle order traversal function.
     * @param tree
     * @param k
     * @return
     */
    public static int kthLargest(TreeNode tree, int k) {
        TreeInfo treeInfo = new TreeInfo(0,-1);
        reverseInOrderTraverse(tree,k,treeInfo);
        return treeInfo.lastestNode;
    }

    private static void reverseInOrderTraverse(TreeNode tree, int k, TreeInfo treeInfo) {
        //If the node of the tree is empty
        //Or return as soon as the current traversal nodes are already k, and will not continue traversing.
        if(null == tree || treeInfo.numberNode >= k){
            return;
        }
        //First traverse the right subtree
        reverseInOrderTraverse(tree.right,k,treeInfo);
        //Determine whether the number of nodes currently traversed is less than k

        if(treeInfo.numberNode < k ){
            //Number of nodes plus 1，
            treeInfo.numberNode += 1;
            //And update the value of the node that was last traversed
            treeInfo.lastestNode = tree.value;
            //Traverse the left subtree again
            reverseInOrderTraverse(tree.left,k,treeInfo);
        }
    }

    public static void main(String[] args) {
        TreeNode tree = new TreeNode(12);

        TreeNode tree2 = new TreeNode(7);
        TreeNode tree3 = new TreeNode(17);

        tree2.left = new TreeNode(3);
        tree2.right = new TreeNode(9);

        tree3.left = new TreeNode(14);

        tree.left = tree2;
        tree.right = tree3;

        System.out.println(kthLargest(tree,2));
    }

}

```