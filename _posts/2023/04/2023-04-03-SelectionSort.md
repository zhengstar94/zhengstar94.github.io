---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Selection Sort"
date: "2023-04-03"
categories:
  - "Algorithm Sorting"
---

# Selection Sort [Easy]

- Write a function that takes in an array of integers and returns a sorted version of that array. Use the Selection Sort algorithm to sort the array.
- If you're unfamiliar with Selection Sort, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

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
    <i><strong> Divide the input array into two subarrays in place.The first subarray should be sorted at all times and should start with a length of 0, while the second subarray should be unsorted. Find the smallest (or largest) element in the unsorted subarray and insert it into the sorted subarray with a swap. Repeat this process of finding the smallest (or largest) element in the unsorted subarray and inserting it in its correct position in the sorted subarray with a swap until the entire array is sorted. </strong></i>
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
 * @date 2023/04/02
 */
public class SelectionSort {
    public static int[] selectionSort(int[] array) {
        int startIdx = 0;
        while (startIdx < array.length - 1) {
            int smallestIdx = startIdx;
            for (int i = startIdx + 1; i < array.length; i++) {
                // Find the index of the smallest element in the unsorted part of the array
                if (array[smallestIdx] > array[i]) {
                    smallestIdx = i;
                }
            }
            // Swap the smallest element with the first element of the unsorted part of the array
            swap(startIdx, smallestIdx, array);
            startIdx++;
        }
        return array;
    }

    private static void swap(int i, int j, int[] array) {
        int temp = array[j];
        array[j] = array[i];
        array[i] = temp;
    }

    public static void main(String[] args) {
        int[] array = new int[]{3,5,1,4,2};
        System.out.println(Arrays.toString(selectionSort(array)));
    }
}

```

