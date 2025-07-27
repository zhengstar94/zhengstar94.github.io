---
toc:
beginning: true
giscus_comments: true
layout: post
title: "2815. Max Pair Sum in an Array"
date: "2025-07-27"
tags: Easy
categories:
    - "LeetCode HashTable"
---


- You are given an integer array `nums`. You have to find the **maximum** sum of a pair of numbers from `nums` such that the **largest digit** in both numbers is equal.
- For example, 2373 is made up of three distinct digits: 2, 3, and 7, where 7 is the largest among them.
- Return the **maximum** sum or -1 if no such pair exists.

**Example 1**

```
Input: nums = [112,131,411]

Output: -1

Explanation:

Each numbers largest digit in order is [2,3,4].
```

**Example 2**

```
Input: nums = [2536,1613,3366,162]

Output: 5902

Explanation:

All the numbers have 6 as their largest digit, so the answer is 2536 + 3366 = 5902.
```

**Example 3**

```
Input: nums = [51,71,17,24,42]

Output: 88

Explanation:

Each number's largest digit in order is [5,7,7,4,4].

So we have only two possible pairs, 71 + 17 = 88 and 24 + 42 = 66.
```

## Method 1

```tex
【O(n * k) time | O(1) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author zhengxingxing
 * @Date 2025/07/27
 */
public class MaxPairSumInAnArray {

    public static int maxSum(int[] nums) {
        // This map stores, for each possible max digit (0~9), the largest number seen so far with that digit.
        Map<Integer, Integer> maxNumForDigit = new HashMap<>();
        int result  = -1; // Initialize result as -1, which will be returned if no valid pair is found.

        for (int num : nums){
            int maxDigit = getMaxDigit(num); // Find the largest digit in the current number.

            // --- Key logic with detailed comments ---
            if (maxNumForDigit.containsKey(maxDigit)){
                // If there is already a number in the map with the same max digit,
                // it means we have found at least one previous number that can form a valid pair with the current number.
                int pairSum = num + maxNumForDigit.get(maxDigit); // Calculate the sum of the current number and the previous max number with the same max digit.
                result = Math.max(result, pairSum); // Update the result if this pairSum is greater than the current result.

                // Update the stored number for this max digit in the map.
                // We always want to keep the largest number seen so far for each max digit,
                // because a larger number may form a larger sum with future numbers.
                maxNumForDigit.put(maxDigit, Math.max(num, maxNumForDigit.get(maxDigit)));
            }else{
                // If this is the first time we see this max digit,
                // just store the current number as the largest number for this digit.
                maxNumForDigit.put(maxDigit, num);
            }
        }

        return result;
    }

    private static int getMaxDigit(int num) {
        int maxDigit = 0;
        while (num > 0) {
            maxDigit = Math.max(maxDigit, num % 10); // Compare the current digit with maxDigit.
            num /= 10; // Move to the next digit.
        }
        return maxDigit;
    }

    public static void main(String[] args) {
        int[] nums1 = {51, 71, 17, 24, 42};
        int[] nums2 = {1, 2, 3, 4};
        int[] nums3 = {12, 21, 33, 39, 93, 99};
        System.out.println(maxSum(nums1)); // Output: 88
        System.out.println(maxSum(nums2)); // Output: -1
        System.out.println(maxSum(nums3)); // Output: 192
    }
}

```





