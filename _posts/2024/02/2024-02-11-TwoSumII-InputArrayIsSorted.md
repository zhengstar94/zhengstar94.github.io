---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "167.Two Sum II - Input Array Is Sorted"
date: "2024-02-11"
categories:
  - "LeetCode TwoPointers"
---

# LeetCode 167. Two Sum II - Input Array Is Sorted [Easy]

- Given a **1-indexed** array of integers `numbers` that is already ***sorted in non-decreasing order\***, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.
- Return *the indices of the two numbers,* `index1` *and* `index2`*, **added by one** as an integer array* `[index1, index2]` *of length 2.*
- The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.
- Your solution must use only constant extra space.

**Example 1**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

**Example 2**

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
```

**Example 3**

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/02/14
 */
public class TwoSumIIInputArrayIsSorted {
    public static int[] twoSum(int[] numbers, int target) {
        // The left pointer starts at the beginning of the array
        int left = 0;
        // The right pointer starts at the end of the array
        int right = numbers.length - 1;
        // Loop until the pointers meet
        while (left < right) {
            // Calculate the sum of the numbers at the current two pointers
            int sum = numbers[left] + numbers[right];
            // If the sum is equal to the target, we found the answer, then return it
            if (sum == target) {
                return new int[] {left + 1, right + 1};
                // If the sum is less than the target, move the left pointer to right to increase the sum
            } else if (sum < target) {
                left++;
                // If the sum is more than the target, move the right pointer to left to decrease the sum
            } else {
                right--;
            }
        }
        // If no answer is found, return [-1, -1] as requested
        return new int[] {-1, -1};
    }

    public static void main(String[] args) {

        // Test case 1: a regular example
        // The expected output is [1, 2] because numbers[1 - 1] + numbers[2 - 1] = 2 + 7 = 9
        System.out.println(Arrays.toString(twoSum(new int[] {2,7,11,15}, 9)));

        // Test case 2: another example
        // The expected output is [1, 3] because numbers[1 - 1] + numbers[3 - 1] = 2 + 4 = 6
        System.out.println(Arrays.toString(twoSum(new int[] {2,3,4}, 6)));

        // Test case 3: an example contains negative numbers
        // The expected output is [1, 2] because numbers[1 - 1] + numbers[2 - 1] = -1 + 0 = -1
        System.out.println(Arrays.toString(twoSum(new int[] {-1,0}, -1)));

        // Test case 4: an example that no answer can be found
        // The expected output is [-1, -1] because no pair of numbers can add up to 10
        System.out.println(Arrays.toString(twoSum(new int[] {2,3,4}, 10)));
    }
}

```

