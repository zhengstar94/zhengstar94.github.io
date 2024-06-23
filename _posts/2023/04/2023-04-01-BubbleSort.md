---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Bubble Sort"
date: "2023-04-01"
categories:
  - "Algorithm Sorting"
---

# Bubble Sort [Easy]

- Write a function that takes in an array of integers and returns a sorted version of that array. Use the Bubble Sort algorithm to sort the array.
- If you're unfamiliar with Bubble Sort, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

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
    <i><strong> Traverse the input array,swapping any two numbers that are out of order and keeping track of any swaps that you make. Once you arrive at the end of the array, check if you have made any swaps; if not, the array is sorted and you are done; otherwise, repeat the steps laid out in this hint until the array is sorted. </strong></i>
</details>

<br>

## Method 1

```tex
【O(n^2)time∣O(1)space】
```

```java
package Sorting;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/03/27
 */
public class BubbleSort {
    /**
     * Bubble sort algorithm that sorts an array of integers in ascending order.
     * It uses a while loop and swaps adjacent elements until the array is sorted.
     *
     * @param array the array of integers to be sorted
     * @return the sorted array of integers
     */
    public static int[] bubbleSort(int[] array) {
        // variable to keep track of whether the array is sorted, initialized to false
        boolean isSorted = false;

        // the length of the unsorted part of the array
        int unsortedLength = array.length - 1;

        // continue sorting until the array is sorted
        while (!isSorted) { 
            // assume the array is sorted, and set to false if a swap occurs
            isSorted = true; 
            for (int i = 0; i < unsortedLength; i++) {

                // swap adjacent elements if they are in the wrong order
                if (array[i] > array[i + 1]) { 
                    // swap the two elements
                    swap(array, i, i + 1); 

                    // set the variable to false to indicate the array is not sorted
                    isSorted = false; 
                }
            }

            // reduce the length of the unsorted part of the array by 1 after each pass
            unsortedLength--; 
        }
        return array;
    }

    /**
     * Swaps two elements in an integer array.
     *
     * @param array the integer array in which the elements are to be swapped
     * @param i the index of the first element to be swapped
     * @param j the index of the second element to be swapped
     */
    public static void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
    
    public static void main(String[] args) {
        int[] array = new int[]{8,5,2,9,5,6,3};
        System.out.println(Arrays.toString(bubbleSort(array)));
    }

}

```

