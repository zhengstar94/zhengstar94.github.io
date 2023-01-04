# BST Construction [Medium]

- Write a `BST` class for a Binary Search Tree.The class should support:

    - Inserting values with the `insert` method
    - Removing values with the `remove` method; this method should only remove the first instance of a given value.
    - Searching for values with the `contains` method.

- Note that you can't remove values from a single-node tree. In other words,calling the `remove` method on a single-node tree should simply not do anything.

- Each `BST` node has an integer `value`,  a `left` child node,and a `right` child node.A node is said to be a valid `BST` node if and only if it satisfies the BST property: its `value` is strictly greater than the values of every node to its left; its `value` is less than or equal to the values of every node to its right; and its children nodes are either valid `BST` nodes themselves or `None` / `null`.



**Sample Usage**

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/08/29/1.png)

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/08/29/2.png)



## Method 1

```tex
Average:\ 【O(log(n))time∣O(1)space】\\
Worst:\ 【O(n)time∣O(1)space】\\
```


```java
package NewArray;

/**
 * @author zhengstars
 * @date 2023/01/03
 */
public class BstConstruction {

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }
  }
  
  public static BST inserValue(BST tree,int target){
    int currentNode = tree.value;

    while(true){
      //if target smaller than currentNode, search to the left
      if(target < currentNode){
        //if tree.left is null, then insert
        if(null == tree.left ){
          tree.left = new BST(target);
          break;
        }else{
          tree = tree.left;
        }
      }else{
        //if target bigger than currentNode, search to the right
        //if tree.right is null, then insert
        if(null == tree.right ){
          tree.right = new BST(target);
          break;
        }else{
          tree = tree.right;
        }
      }
    }

    return tree;
  }


  public static boolean containsValu(BST tree,int target){
    int currentNode = tree.value;

    // if tree is not null
    while(null != tree){
      if(target < currentNode){
        tree = tree.left;
      }else if(target > currentNode){
        tree = tree.right;
      }else{
        return true;
      }
    }
    return false;
  }


  public static BST remove(BST node, int value){
    // If no node is found, return
    if (node == null) {
      return null;
    }

    // If the value you want to delete is smaller than the value of the current node, delete it in the left subtree
    if (value < node.value) {
      node.left = remove(node.left, value);
    }
    // If the value you want to delete is larger than the value of the current node, delete it in the right subtree
    else if (value > node.value) {
      node.right = remove(node.right, value);
    }else {
      // If the node does not have child nodes, delete it
      if (node.left == null && node.right == null) {
        node = null;
      }
      // If the node has only the left child, delete it and replace it with the left child
      else if (node.right == null) {
        node = node.left;
      }
      // If the node has only the right child node, delete it and replace it with the right child node
      else if (node.left == null) {
        node = node.right;
      }
      // If the node has two child nodes, find the minimum value in the right subtree,
      // assign its value to the current node, and delete the node where the minimum value is located
      else {
        BST min = findMin(node.right);
        node.value = min.value;
        node.right = remove(node.right, min.value);
      }
    }
    return node;
  }

  public static BST findMin(BST tree){
    while(null != tree.left){
      tree = tree.left;
    }
    return tree;
  }

  public static void main(String[] args) {
    BST tree = new BST(10);

    BST tree1 = new BST(2);
    tree1.left = new BST(1);

    BST tree2 = new BST(5);
    tree2.left = tree1;
    tree2.right = new BST(6);

    BST tree3 = new BST(13);
    tree3.right = new BST(14);

    BST tree4 = new BST(15);
    tree4.right = new BST(22);
    tree4.left = tree3;

    tree.left = tree2;
    tree.right = tree4;

    BST root = remove(tree,10);
    System.out.println(root);

  }

}

```

