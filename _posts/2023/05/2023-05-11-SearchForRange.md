---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Search For Range"
date: "2023-05-11"
categories:
  - "Algorithm Searching"
---

# Search For Range [Hard]

- Write a function that takes in a sorted array of integers as well as a target integer. The function should use a variation of the Binary Search algorithm to find a range of indices in between which the target number is contained in the array and should return this range in the form of an array.

- The first number in the output array should represent the first index at which the target number is located, while the second number should represent the last index at which the target number is located. The function should return [-1, -1] if the integer isn't contained in the array.

- If you're unfamiliar with Binary Search, we recommend watching the Conceptual Overview section of the Binary Search question's video explanation before starting to code.



**Sample Input **

```
array = [0, 1, 21, 33, 45, 45, 45, 45, 45, 45, 61, 71, 73]
target = 45
```

**Sample Output **

```
[4, 9]
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>The Binary Search algorithm involves a left pointer and a right pointer and using those pointers to find the middle number in an array in an effort to find a target number. Unlike with normal Binary Search however, here you cannot simply find the middle number of the array, compare it to the target, and stop once you find it because you are looking for a range rather than a single number. Instead, realize that whenever you find the middle number in the array, the following two scenarios are possible: either the middle number is not equal to the target number, in which case you must proceed with normal Binary Search, or the middle number is equal to the target number, in which case you must figure out if this middle number is an extremity of the range or not. </strong></i>
</details>







<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Try applying an altered version of Binary Search twice: once to find the left extremity of the range and once to find the right extremity of the range. How can you accomplish this? What are the time complexity implications of this approach? </strong></i>
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

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/06/10
 */
public class SearchForRange {
    public static int[] searchForRange(int[] array, int target) {
        // Helper function to find the left index of the target
        int leftIdx = findLeftIndex(array, target);
        // Helper function to find the right index of the target
        int rightIdx = findRightIndex(array, target);
        return new int[]{leftIdx, rightIdx};
    }

    private static int findLeftIndex(int[] array, int target) {
        int left = 0;
        int right = array.length - 1;
        int result = -1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (array[mid] >= target) {
                right = mid - 1;
                if (array[mid] == target){
                    result = mid;
                }
            } else {
                left = mid + 1;
            }
        }

        return result;
    }

    private static int findRightIndex(int[] array, int target) {
        int left = 0;
        int right = array.length - 1;
        int result = -1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (array[mid] <= target) {
                left = mid + 1;
                if (array[mid] == target){
                    result = mid;
                }
            } else {
                right = mid - 1;
            }
        }

        return result;
    }

    public static void main(String[] args) {
        int[] array = {0, 1, 21, 33, 45, 45, 45, 45, 45, 45, 61, 71, 73};
        int target = 45;
        int[] result = searchForRange(array, target);
        System.out.println(Arrays.toString(result));
    }
}

```

