---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1338. Reduce Array Size to The Half"
date: "2024-12-15"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given an integer array `arr`. You can choose a set of integers and remove all the occurrences of these integers in the array.
- Return *the minimum size of the set so that **at least** half of the integers of the array are removed*.

**Example 1**

```
Input: arr = [3,3,3,3,5,5,5,2,2,7]
Output: 2
Explanation: Choosing {3,7} will make the new array [5,5,5,2,2] which has size 5 (i.e equal to half of the size of the old array).
Possible sets of size 2 are {3,5},{3,2},{5,2}.
Choosing set {2,7} is not possible as it will make the new array [3,3,3,3,5,5,5] which has a size greater than half of the size of the old array.
```

**Example 2**

```
Input: arr = [7,7,7,7,7,7]
Output: 1
Explanation: The only possible set you can choose is {7}. This will make the new array empty.
```

## Method 1

```tex
【O(nlog(n) time | O(m) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/15
 */
public class ReduceArraySizeToTheHalf {
    public static int minSetSize(int[] arr) {
        // Create a frequency array to count occurrences of each number
        // Size 10001 to accommodate the problem's given number range
        int[] nums = new int[10001];

        // Count the frequency of each number in the input array
        // For each number, increment its count in the frequency array
        for (int i = 0; i < arr.length; i++) {
            nums[arr[i]]++;
        }

        // Sort the frequency array in ascending order
        // This allows us to process numbers from highest to lowest frequency
        Arrays.sort(nums);

        // Variable to track the count of elements removed
        int cnt = 0;

        // Iterate from the end of the array (highest frequencies)
        // Work backwards to minimize the number of unique elements removed
        for (int i = nums.length; i > 0 ; i--) {
            // Add the current frequency to the total count of removed elements
            cnt = cnt + nums[i - 1];

            // Check if the removed elements exceed half the original array length
            // If so, return the number of unique elements removed
            if(cnt >= arr.length/2){
                // Calculate the number of unique elements removed
                // nums.length - i + 1 gives the count of frequency groups removed
                return nums.length - i + 1;
            }
        }

        // If no solution is found, return the entire array length
        // This is a fallback scenario, though unlikely to occur given the problem constraints
        return arr.length;
    }

    public static void main(String[] args) {
        // Test Case 1: Array with different elements
        // Demonstrates removing elements to reduce array size
        int[] arr1 = {3, 3, 3, 3, 5, 5, 5, 2, 2, 7};
        System.out.println("Test Case 1 Result: " + minSetSize(arr1)); // Expected output: 2

        // Test Case 2: Array with all same elements
        // Shows handling of uniform frequency elements
        int[] arr2 = {7, 7, 7, 7, 7, 7};
        System.out.println("Test Case 2 Result: " + minSetSize(arr2)); // Expected output: 1

        // Test Case 3: More complex array with varied frequencies
        // Tests scenario with multiple unique elements
        int[] arr3 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4};
        System.out.println("Test Case 3 Result: " + minSetSize(arr3)); // Expected output: 4

        // Test Case 4: Large data range
        // Verifies the solution works with a larger, more diverse input
        int[] arr4 = {9,77,63,22,92,9,14,54,8,38,18,64,92,53,5,57,7,76,40,5};
        System.out.println("Test Case 4 Result: " + minSetSize(arr4)); // Expected output result
    }
}

```





