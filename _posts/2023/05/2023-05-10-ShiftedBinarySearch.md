---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Shifted Binary Search"
date: "2023-05-10"
categories:
  - "Algorithm Searching"
---

# Shifted Binary Search [Hard]

- Write a function that takes in a sorted array of distinct integers as well as a target integer. The caveat is that the integers in the array have been shifted by some amount; in other words, they've been moved to the left or to the right by one or more positions. For example, [1, 2, 3, 4] might have turned into [3, 4, 1, 2].

- The function should use a variation of the Binary Search algorithm to determine if the target integer is contained in the array and should return its index if it is, otherwise -1.

- If you're unfamiliar with Binary Search, we recommend watching the Conceptual Overview section of the Binary Search question's video explanation before starting to code.



**Sample Input **

```
array = [45, 61, 71, 72, 73, 0, 1, 21, 33, 37]
target = 33
```

**Sample Output **

```
8
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>The Binary Search algorithm involves a left pointer and a right pointer and using those pointers to find the middle number in an array. Unlike with a normal sorted array however, you cannot simply find the middle number of the array and compare it to the target here, because the shift could lead you in the wrong direction. Instead, realize that whenever you find the middle number in the array, the following two scenarios are possible (assuming the middle number is not equal to the target number, in which case you're done): either the left-pointer number is smaller than or equal to the middle number, or it is bigger. Figure out a way to eliminate half of the array depending on the scenario. </strong></i>
</details>






<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>In the scenario where the left-pointer number is smaller than or equal to the middle number, two other scenarios can arise: either the target number is smaller than the middle number and greater than or equal to the left-pointer number, or it's not. In the first scenario, the right half of the array can be eliminated; in the second scenario, the left half can be eliminated. Figure out the scenarios that can arise if the left-pointer number is greater than the middle number and apply whatever logic you come up with recursively until you find the target number or until you run out of numbers in the array. </strong></i>
</details>


<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Can you implement this algorithm iteratively? Are there any advantages to doing so? </strong></i>
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
 * @date 2023/06/09
 */
public class ShiftedBinarySearch {
    public static int shiftedBinarySearch(int[] array, int target) {
        // left boundary pointer
        int left = 0;
        // right boundary pointer
        int right = array.length - 1;

        // perform the loop as long as the left boundary is less than or equal to the right boundary
        while (left <= right) {
            // calculate the index of the middle element
            int mid = left + (right - left) / 2;

            // if the middle element is equal to the target value, return the index
            if (array[mid] == target) { 
                return mid;
            }

            // if the left half is sorted
            if (array[left] <= array[mid]) {
                // target value is in the left half
                if (target >= array[left] && target < array[mid]) { // target value is within the range of the left half
                    // update the right boundary pointer
                    right = mid - 1; 
                } else {
                    // update the left boundary pointer
                    left = mid + 1; 
                }
            }
            // if the right half is sorted
            else {
                // target value is in the right half
                if (target > array[mid] && target <= array[right]) { // target value is within the range of the right half
                    // update the left boundary pointer
                    left = mid + 1; 
                } else {
                    // update the right boundary pointer
                    right = mid - 1; 
                }
            }
        }

        // target value does not exist in the array
        return -1; 
    }

    public static void main(String[] args) {
        int[] array = {45, 61, 71, 72, 73, 0, 1, 21, 33, 37};
        int target = 33;
        int result = shiftedBinarySearch(array, target);
        System.out.println(result);
    }
}

```

