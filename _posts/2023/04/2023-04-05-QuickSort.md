---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Quick Sort"
date: "2023-04-05"
categories:
  - "Algorithm Sorting"
---

# Quick Sort [Hard]

- Write a function that takes in an array of integers and returns a sorted version of that array. Use the Quick Sort algorithm to sort the array.
- If you're unfamiliar with Quick Sort, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

**Sample Input**

```
array=[8,5,2,9,5,6,3]
```

**Sample Output**

```
[2,3,5,5,6,8,9]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Quick Sort works by picking a "pivot" number from an array, positioning every other number in the array in sorted order with respect to the pivot (all smaller numbers to the pivot's left; all bigger numbers to the pivot's right), and then repeating the same two steps on both sides of the pivot until the entire array is sorted. </strong></i>
</details>


<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Pick a random number from the input array (the first number, for instance) and let that number be the pivot. Iterate through the rest of the array using two pointers, one starting at the left extremity of the array and progressively moving to the right, and the other one starting at the right extremity of the array and progressively moving to the left. As you iterate through the array,compare the left and right pointer numbers to the pivot. If the left number is greater than the pivot and the right number is less than the pivot, swap them; this will effectively sort these numbers with respect to the pivot at the end of the iteration. If the left number is ever less than or equal to the pivot, increment the left pointer; similarly, if the right number is ever greater than or equal to the pivot, decrement the right pointer. Do this until the pointers pass each other, at which point swapping the pivot with the right number should position the pivot in its final,sorted position, where every number to its left is smaller and every number to its right is greater. </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Repeat the process mentioned in Hint #2 on the respective subarrays located to the left and right of your pivot, and keep on repeating the process thereafter until the input array is fully sorted. </strong></i>
</details>


<br>

## Method 1

```tex
【O(nlogn)time∣O(logn)space】
```

```java
package Sorting;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/04/08
 */
public class QuickSort {
    public static int[] quickSort(int[] array) {
        quickSortHelper(array, 0, array.length - 1);
        return array;
    }

    private static void quickSortHelper(int[] array, int left, int right) {
        if (left >= right) {
            return;
        }
        // Get the index of the pivot element
        int pivotIndex = left + (right - left) / 2;
        // Get the value of the pivot element
        int pivot = array[pivotIndex];
        // Initialize the left pointer
        int i = left;
        // Initialize the right pointer
        int j = right;
        // Loop until the left and right pointers meet
        while (i <= j) {
            // Find the first element from the left that is greater than or equal to the pivot
            while (array[i] < pivot) {
                i++;
            }
            // Find the first element from the right that is less than or equal to the pivot
            while (array[j] > pivot) {
                j--;
            }
            // If the left pointer is still less than or equal to the right pointer
            if (i <= j) {
                // Swap the two elements
                swap(array, i, j);
                // Move the left pointer to the right
                i++;
                // Move the right pointer to the left
                j--;
            }
        }
        // Recursively sort the left half
        quickSortHelper(array, left, j);
        // Recursively sort the right half
        quickSortHelper(array, i, right);
    }

    private static void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }

    public static void main(String[] args) {
        int[] array = new int[]{1,5,3,2,4};
        System.out.println(Arrays.toString(quickSort(array)));
    }

}

```

