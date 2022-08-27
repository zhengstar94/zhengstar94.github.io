# Find Closest Value In BST [Easy]

- Write a function that takes in a Binary Search Tree (BST)and a target integer value and returns the closest value to that target value contained in the BST.

- You can assume that there will only be one closest value.

- Each `BST` node has an integer `value`,  a `left` child node, and a `right` child node. A node is said to be a valid `BST` node if and only if it satisfies the BST property: its `value` is strictly greater than the values of every node to its left; its `value` is less than or equal to the values of every node to its right; and its children nodes are either valid `BST` nodes themselves or `None`/ `null`.



**Sample Input #1**

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/08/26/1.png)

**Sample Output #1**

> 13

Method 1(Recursive)

```tex
Average:\ 【O(log(n))time∣O(log(n))space】\\
Worst:\ 【O(n)time∣O(n)space】\\
```


```java
import java.util.*;

class Program {
  public static int findClosestValueInBst(BST tree, int target) {
      return findClosestValueInBstHelper(tree,target,tree.value);
  }

  private static int findClosestValueInBstHelper(BST tree, int target, int closest) {
      if(null == tree){
          return closest;
      }

      // Find the value closest to the target
      if(Math.abs(target - closest) > Math.abs(target - tree.value) ){
          closest = tree.value;
      }

      if(target < tree.value){
          return findClosestValueInBstHelper(tree.left,target,closest);
      }else if(target > tree.value){
          return findClosestValueInBstHelper(tree.right,target,closest);
      }else{
          return closest;
      }

  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }
  }
}
```







