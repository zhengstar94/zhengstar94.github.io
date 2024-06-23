---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Insertion Sort"
date: "2023-04-02"
categories:
  - "Algorithm Sorting"
---

# Insertion Sort [Easy]

- Write a function that takes in an array of integers and returns a sorted version of that array. Use the Insertion Sort algorithm to sort the array.
- If you're unfamiliar with Insertion Sort, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

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
    <i><strong> Divide the input array into two subarrays in place.The first subarray should be sorted at all times and should start with a length of 1, while the second subarray should be unsorted. Iterate through the unsorted subarray, inserting all of its elements into the sorted subarray in the correct position by swapping them into place. Eventually,the entire array will be sorted. </strong></i>
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
public class InsertionSort {
    public static int[] insertionSort(int[] array) {
        // Traverse the entire array
        for (int i = 1; i < array.length; i++) {
            // Stores the location of the current element in j
            int j = i;
            // If the current element is smaller than the previous element, swap their positions
            while (j > 0 && array[j] < array[j - 1]) {
                // Call the swap function to exchange elements
                swap(j, j - 1, array);
                // Move forward the position of j
                j--;
            }
        }
        // Returns the sorted array
        return array;
    }

    // Swap the position of two elements in an array
    public static void swap(int i, int j, int[] array) {
        int temp = array[j];
        array[j] = array[i];
        array[i] = temp;
    }

    public static void main(String[] args) {
        int[] array = new int[]{3,5,1,4,2};
        System.out.println(Arrays.toString(insertionSort(array)));
    }
}

```

