---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Quick Select"
date: "2023-05-12"
categories:
  - "Algorithm Searching"
---

# Quick Select [Hard]

- Write a function that takes in an array of distinct integers as well as an integer k and that returns the kth smallest integer in that array.
- The function should do this in linear time, on average.

**Sample Input **

```
array = [8, 5, 2, 9, 7, 6, 3]
k = 3
```

**Sample Output **

```
5
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>The Quick Sort sorting algorithm works by picking a "pivot" number from an array, positioning every other number in the array in sorted order with respect to the pivot (all smaller numbers to the pivot's left; all bigger numbers to the pivot's right), and then repeating the same two steps on both sides of the pivot until the entire array is sorted. Apply the technique used in Quick Sort until the pivot element gets positioned in the kth place in the array, at which point you'll have found the answer to the problem. </strong></i>
</details>







<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Pick a random number from the input array (the first number, for instance) and let that number be the pivot. Iterate through the rest of the array using two pointers, one starting at the left extremity of the array and progressively moving to the right, and the other one starting at the right extremity of the array and progressively moving to the left. As you iterate through the array, compare the left and right pointer numbers to the pivot. If the left number is greater than the pivot and the right number is less than the pivot, swap them; this will effectively sort these numbers with respect to the pivot at the end of the iteration. If the left number is ever less than or equal to the pivot, increment the left pointer; similarly, if the right number is ever greater than or equal to the pivot, decrement the right pointer. Do this until the pointers pass each other, at which point swapping the pivot with the right number should position the pivot in its final, sorted position, where every number to its left is smaller and every number to its right is greater. If the pivot is in the kth position, you're done; if it isn't, figure out if the kth smallest number is located to the left or to the right of the pivot. </strong></i>
</details>



<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Repeat the process mentioned in Hint #2 on the side of the kth smallest number, and keep on repeating the process thereafter until you find the answer. What is the time complexity of this algorithm? </strong></i>
</details>



<br>



## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Searching;

/**
 * @author zhengstars
 * @date 2023/06/10
 */
public class Quickselect {
    public static int quickselect(int[] array, int k) {
        int left = 0;
        int right = array.length - 1;

        while (true) {
            // Partition the array and get the pivot index
            int pivotIndex = partition(array, left, right);

            // If the pivot index is equal to k-1, we found the kth smallest element
            if (pivotIndex == k - 1) {
                return array[pivotIndex];
            }
            // If the pivot index is greater than k-1, search in the left subarray
            else if (pivotIndex > k - 1) {
                right = pivotIndex - 1;
            }
            // If the pivot index is less than k-1, search in the right subarray
            else {
                left = pivotIndex + 1;
            }
        }
    }

    private static int partition(int[] array, int left, int right) {
        int pivot = array[right];
        int i = left - 1;

        // Perform partitioning
        for (int j = left; j < right; j++) {
            if (array[j] <= pivot) {
                i++;
                swap(array, i, j);
            }
        }

        // Move the pivot element to its correct position
        swap(array, i + 1, right);
        return i + 1;
    }

    private static void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }

    public static void main(String[] args) {
        int[] array = {8, 7, 5, 3, 2};
        int k = 3;
        int result = quickselect(array, k);
        System.out.println(result);
    }
}

```

