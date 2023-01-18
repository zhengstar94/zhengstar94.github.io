# Same BSTs

- An array of integers is said to represent the Binary Search Tree (BST) obtained by inserting each integer in the array, from left to right, into the BST.

- Write a function that takes in two arrays of integers and determines whether these arrays represent the same BST. Note that you're not allowed to construct any BSTs in your code.

- A BST is a Binary Tree that consists only of BST nodes. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.

  **Sample Input **

```
arrayOne = [10, 15, 8, 12, 94, 81, 5, 2, 11]
arrayTwo = [10, 8, 5, 15, 2, 12, 11, 94, 81]
```

**Sample Output **

```
true // both arrays represent the BST below
         10
       /     \
      8      15
    /       /   \
   5      12    94
 /       /     /
2       11    81
```



## Method 1(Recursion)

```tex
【O(n^2)time∣O(n^2)space】
```

```java
package BST;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/01/17
 */
public class SameBSTs {
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

    public static boolean isSameBST(List<Integer> arrayOne, List<Integer> arrayTwo){
       // if arrays are null, they are the same BST
       if(arrayOne.size() == 0 && arrayTwo.size() == 0){
           return true;
       }

       // Edge case: if the arrays are of different length, they cannot be the same BST
       if(arrayOne.size() != arrayTwo.size()){
           return false;
       }

       // If the root nodes are not equal, the BSTs are not the same
       if(!arrayOne.get(0).equals(arrayTwo.get(0))){
           return false;
       }

       // Get a collection smaller than the root node
       List<Integer> leftOne = getSmall(arrayOne);
       List<Integer> leftTwo = getSmall(arrayTwo);

        // Get a collection bigger than the root node
       List<Integer> rightOne = getBiggerOrEqual(arrayOne);
       List<Integer> rightTwo = getBiggerOrEqual(arrayTwo);

       // Iteration
       return isSameBST(leftOne,leftTwo) && isSameBST(rightOne,rightTwo);
    }

    /**
     * Get an array smaller than the root node
     * @param array
     * @return
     */
    private static List<Integer> getSmall(List<Integer> array) {
        List<Integer> smaller = new ArrayList<>();
        for (int i = 1; i < array.size(); i++) {
            if (array.get(i) < array.get(0)) {
                smaller.add(array.get(i));
            }
        }
        return smaller;
    }

    /**
     * Get an array bigger than the root node
     * @param array
     * @return
     */
    private static List<Integer> getBiggerOrEqual(List<Integer> array) {
        List<Integer> bigger = new ArrayList<>();
        for (int i = 1; i < array.size(); i++) {
            if (array.get(i) >= array.get(0)) {
                bigger.add(array.get(i));
            }
        }
        return bigger;
    }

    public static void main(String[] args) {
        Integer[] array1= new Integer[]{10, 15, 8, 12, 94, 81, 5, 2, 11};
        Integer[] array2= new Integer[]{10, 8, 5, 15, 2, 12, 11, 94, 81};

        System.out.println(isSameBST(Arrays.asList(array1),Arrays.asList(array2)));
    }
}

```

## Method 2

```tex
【O(n^2)time∣O(n)space】
```

```java
package BST;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/01/17
 */
public class SameBSTs {
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

    public static boolean sameBST(List<Integer> arrayOne, List<Integer> arrayTwo) {
        // call the recursive function to determine whether the two arrays are the same BST
        return areSameBst (arrayOne, arrayTwo, 0, 0, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private static boolean areSameBst(List<Integer> arrayOne, List<Integer> arrayTwo, int rootIdOne, int rootIdTwo, long minValue, long maxValue) {
        // if both rootIdOne and rootIdTwo are-1, true is returned
        if(rootIdOne == -1 || rootIdTwo == -1){
            return rootIdOne == rootIdTwo;
        }

        // Returns false if the values in the two arrays are not equal
        if(!arrayOne.get(rootIdOne).equals(arrayTwo.get(rootIdTwo)) ){
            return false;
        }

        // Find the left child node of rootIdOne in arrayOne
        int leftRoodIdOne = getIdxOfFirstSmaller(arrayOne,rootIdOne,minValue);
        // Find the left child node of rootIdTwo in arrayTwo
        int leftRoodIdTwo = getIdxOfFirstSmaller(arrayTwo,rootIdTwo,minValue);

        // Find the right child node of rootIdOne in arrayOne
        int rightRoodIdOne = getIdxOfFirstBiggerOrEqual(arrayOne,rootIdOne,maxValue);
        // Find the right child node of rootIdTwo in arrayTwo
        int rightRoodIdTwo = getIdxOfFirstBiggerOrEqual(arrayTwo,rootIdTwo,maxValue);

        // Value of the current node
        int CurrentValue = arrayOne.get(rootIdOne);

        // Recursively judge whether the left subtree is equal or not
        boolean leftAreSame = areSameBst(arrayOne, arrayTwo, leftRoodIdOne, leftRoodIdTwo, minValue, CurrentValue);
        // Recursively judge whether the right subtree is equal or not
        boolean rightAreSame = areSameBst(arrayOne, arrayTwo, rightRoodIdOne, rightRoodIdTwo, CurrentValue, maxValue);

        // Returns true if both left and right subtrees are equal
        return leftAreSame && rightAreSame;
    }

    /**
     * Find the subscript of the first element that is less than array.get (startingIdx) and greater than or equal to minValue
     * @param array
     * @param startingIdx
     * @param minValue
     * @return
     */
    private static int getIdxOfFirstSmaller(List<Integer> array, int startingIdx, long minValue) {
        for (int i = startingIdx + 1; i < array.size(); i++) {
            if(array.get(i) < array.get(startingIdx) && array.get(i) >= minValue){
                return i;
            }
        }
        return -1;
    }

    /**
     * Find the subscript of the first element greater than or equal to array.get (startingIdx) and less than maxValue
     * @param array
     * @param startingIdx
     * @param maxValue
     * @return
     */
    private static int getIdxOfFirstBiggerOrEqual(List<Integer> array, int startingIdx, long maxValue) {
        for (int i = startingIdx + 1; i < array.size(); i++) {
            if(array.get(i) >= array.get(startingIdx) && array.get(i) < maxValue){
                return i;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        Integer[] array1= new Integer[]{10, 15, 8, 12, 94, 81, 5, 2, 11};
        Integer[] array2= new Integer[]{10, 8, 5, 15, 2, 12, 11, 94, 81};

        System.out.println(sameBST(Arrays.asList(array1),Arrays.asList(array2)));
        //System.out.println(isSameBST(Arrays.asList(array1),Arrays.asList(array2)));
    }

}

```

