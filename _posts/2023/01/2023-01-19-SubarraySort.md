---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Subarray Sort"
date: "2023-01-19"
categories:
  - "Algorithm Array"
---


# Subarray Sort [Hard]

- Write a function that takes in an array of at least two integers and that returns an array of the starting and ending indices of the smallest subarray in the input array that needs to be sorted in place in order for the entire input array to be sorted (in ascending order).
- If the input array is already sorted,the function should return` [-1,-1]`.

**Sample Input #1**

> array=[1,2,4,7,10,11,7,12,6,7,16,18,19]

**Sample Output #1**

> [3,9]

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Realize that even a single out-of-order number in the input array can call for a large subarray to have to be sorted.This is because,depending on how out-of-place the number is,it might need to be moved very far away from its original position in order to be in its sorted position. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Find the smallest and largest numbers that are out of order in the input array.You should be able to do this in a single pass through the array.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Once you've found the smallest and largest out-of-order numbers mentioned in Hint #2,find their final sorted positions in the array.This should give you the extremities of the smallest subarray that needs to be sorted.  </strong></i>
</details>

<br>

## Method 1

```tex
【 O(n)time | O(1) space 】
```



```java
import java.util.*;

class Program {
  public static int[] subarraySort(int[] array) {
      // Write your code here.
      if (array == null || array.length < 2) {
          return new int[] {};
      }
      int minOutOfNum = Integer.MAX_VALUE;
      int maxOutOfNum = Integer.MIN_VALUE;

      for (int i = 0; i < array.length; i++) {
          if (isOutOfOrder(i, array[i], array)) {
              //find the maximum and minimum values in disordered list
              minOutOfNum = Math.min(minOutOfNum, array[i]);
              maxOutOfNum = Math.max(maxOutOfNum, array[i]);
          }
      }

      //if minOutOfNum don't change,then the array is sort，return[-1, -1]
      if (minOutOfNum == Integer.MAX_VALUE) {
          return new int[]{-1, -1};
      }

      //find the minimum of the array from left to right
      int leftIdx = 0;
      while (minOutOfNum >= array[leftIdx]) {
          leftIdx++;
      }

      //find the maximum of the array from right to left
      int rightIdx = array.length - 1;
      while (maxOutOfNum <= array[rightIdx]) {
          rightIdx--;
      }

      return new int[] {leftIdx, rightIdx};
  }

  public static boolean isOutOfOrder(int i, int num, int[] array) {
      //if the index is the first number, and if num > array[i + 1], then num is disordered number
      if (i == 0) {
          return num > array[i + 1];
      }

      //if the index is the last number,if num < array[i - 1], then num is disordered number
      if (i == array.length - 1) {
          return num < array[i - 1];
      }
      //if num > array[i + 1] or num < array[i - 1] ,then num is disordered number and need return true
      return num > array[i + 1] || num < array[i - 1];
  }
  
}
```
