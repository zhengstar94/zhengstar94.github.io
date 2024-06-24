---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Binary Search"
date: "2023-05-07"
categories:
  - "Algorithm Searching"
---

# Binary Search [Easy]

- Write a function that takes in a sorted array of integers as well as a target integer. The function should use the Binary Search algorithm to determine if the target integer is contained in the array and should return its index if it is, otherwise -1.
- If you're unfamiliar with Binary Search, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

**Sample Input **

```
array = [0, 1, 21, 33, 45, 45, 61, 71, 72, 73]
target = 33
```

**Sample Output **

```
3
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>The Binary Search algorithm works by finding the number in the middle of the input array and comparing it to the target number. Given that the array is sorted, if this middle number is smaller than the target number, then the entire left part of the array is no longer worth exploring since the target number can no longer be in it; similarly, if the middle number is greater than the target number, then the entire right part of the array is no longer worth exploring. Applying this logic recursively eliminates half of the array until the number is found or until the array runs out of numbers. </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Write a helper function that takes in two additional arguments: a left pointer and a right pointer representing the indices at the extremities of the array (or subarray) that you are applying Binary Search on. The first time this helper function is called, the left pointer should be zero and the right pointer should be the final index of the input array. To find the index of the middle number mentioned in Hint #1, simply round down the number obtained from: (left + right) / 2. Apply this logic recursively until you find the target number or until the left pointer becomes greater than the right pointer. </strong></i>
</details>





<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Can you implement this algorithm iteratively? Are there any advantages to doing so?</strong></i>
</details>



<br>

## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Searching;

/**
 * @author zhengstars
 * @date 2023/06/06
 */
public class BinarySearch {
    public static int binarySearch(int[] array, int target) {
        // Initialize the left pointer to the start of the array
        int left = 0;
        // Initialize the right pointer to the end of the array
        int right = array.length - 1;

        // Perform binary search until the left and right pointers meet
        while (left <= right) {
            // Calculate the middle index
            int middle = left + (right - left)/2;

            // If the middle element is equal to the target
            if (array[middle] == target) {
                // Return the index of the target element
                return middle;
            } else if (array[middle] < target) {  // If the middle element is less than the target
                // Update the left pointer to search in the right half
                left = middle + 1;
            } else {                             // If the middle element is greater than the target
                // Update the right pointer to search in the left half
                right = middle - 1;
            }
        }

        return -1;                     // Return -1 if the target element is not found
    }

    public static void main(String[] args) {
        int[] array = {0, 1, 21, 33, 45, 45, 61, 71, 72, 73};
        int target = 33;
        int result = binarySearch(array, target);
        System.out.println(result);
    }
}

```

