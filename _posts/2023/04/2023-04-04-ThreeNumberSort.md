---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Three Number Sort"
date: "2023-04-04"
categories:
  - "Algorithm Sorting"
---

# Three Number Sort [Medium]

- You're given an array of integers and another array of three distinct integers. The first array is guaranteed to only contain integers that are in the second array, and the second array represents a desired order for the integers in the first array. For example, a second array of `[x,y,z]` represents a desired order of `[x,x,...x,y,y,...y,z,z,...z]`in the first array.

- Write a function that sorts the first array according to the desired order in the second array.

- The function should perform this in place (i.e., it should mutate the input array), and it shouldn't use any auxiliary space (i.e., it should run with constant space: `0(1)` space).

- Note that the desired order won't necessarily be ascending or descending and that the first array won't necessarily contain all three integers found in the second array-it might only contain one or two.



**Sample Input**

```
array = [1, 0, 0, -1, -1, 0, 1, 1] 
order = [0, 1, -1]
```

**Sample Output**

```
[0,0,0,1,1,1,-1,-1]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> What advantage does knowing the three values contained in the array give you, and how can you use that to solve this problem in linear time?</strong></i>
</details>


<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Try counting how many times each of the three values appears in the input array. Once you have these counts, you can repopulate the input array as need be. </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Putting aside the first two hints, try conceptually splitting the original array into three subarrays and moving elements of each unique value into the correct subarray. You'll need to keep track of the respective starting indices of these subarrays. </strong></i>
</details>


<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Going off of Hint #3, you can solve this problem either with two passes through the input array or with a single pass. If you do two passes through the array, you'll specifically be positioning the first ordered element during the first pass and the third ordered element during the second pass. You'll be swapping elements from the left side of the array whenever you encounter the first element, and you'll be swapping elements from the right side of the array whenever you encounter the third element. You'll have to keep track of where you last placed a first element or a third element. With a single pass through the array, you'll have to implement both of these strategies and a little more all at once. </strong></i>
</details>


<br>

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Sorting;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/04/05
 */
public class ThreeNumberSort {
    public static int[] threeNumberSort(int[] array, int[] order) {
        // Initialize the index of the first element of the sorted array
        int firstIdx = 0;
        // Initialize the index of the second element of the sorted array
        int secondIdx = 0;
        // Initialize the index of the third element of the sorted array
        int thirdIdx = array.length - 1;
        // Repeat until secondIdx is greater than thirdIdx
        while (secondIdx <= thirdIdx) {
            // Get the value of the element at the current index
            int value = array[secondIdx];
            // If the element is equal to the first element of the order
            if (value == order[0]) {
                // Swap the element with the first element of the sorted array
                swap(array, firstIdx, secondIdx);
                // Increment the index of the first element of the sorted array
                firstIdx++;
                // Increment the index of the second element of the sorted array
                secondIdx++; 
            } else if (value == order[1]) { // If the element is equal to the second element of the order
                // Increment the index of the second element of the sorted array
                secondIdx++; 
            } else { // If the element is equal to the third element of the order
                // Swap the element with the third element of the sorted array
                swap(array, secondIdx, thirdIdx);
                // Decrement the index of the third element of the sorted array
                thirdIdx--; 
            }
        }
        // Return the sorted array
        return array; 
    }

    /**
     * Define a helper method to swap two elements in the array
     * @param array
     * @param i
     * @param j
     */
    private static void swap(int[] array, int i, int j) { 
        int temp = array[i]; 
        array[i] = array[j]; 
        array[j] = temp; 
    }

    public static void main(String[] args) {
        int[] array = new int[]{1, 0, 0, -1, -1, 0, 1, 1};
        int[] order = new int[]{0, 1, -1};

        System.out.println(Arrays.toString(threeNumberSort(array,order)));
    }
}

```

