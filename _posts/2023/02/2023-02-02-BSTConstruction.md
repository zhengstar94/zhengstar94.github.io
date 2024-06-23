---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "BST Construction"
date: "2023-02-02"
categories:
  - "Algorithm BST"
---

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


**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> As you try to insert,find,or a remove a value into,in,or from a BST,you will have to traverse the tree's nodes.The BST property allows you to eliminate half of the remaining tree at each node that you traverse:if the target value is strictly smaller than a node's value,then it must be (or can only be)located to the left of the node,otherwise it must be (or can only be)to the right of that node. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Traverse the BST all the while applying the logic described in Hint #1.For insertion,add the target value to the BST once you reach a leaf (None null)node.For searching,if you reach a leaf node without having found the target value that means the value isn't in the BST.For removal,consider the various cases that you might encounter:the node you need to remove might have two children nodes,one,or none;it might also be the root node;make sure to account for all of these cases.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> What are the advantages and disadvantages of implementing these methods iteratively as opposed to recursively?  </strong></i>
</details>

<br>

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

  public static BST inserValue(BST root, int target){
    if (root == null) {
      return new BST(target);
    }
    // Define global variables
    BST tree = root;

    while(true){
      if(target < tree.value){
        if(null == tree.left){
          tree.left = new BST(target);
          break;
        }else{
          tree =  tree.left;
        }
      }else{
        if(null == tree.right){
          tree.right = new BST(target);
          break;
        }else{
          tree =  tree.right;
        }
      }
    }
    return root;
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

