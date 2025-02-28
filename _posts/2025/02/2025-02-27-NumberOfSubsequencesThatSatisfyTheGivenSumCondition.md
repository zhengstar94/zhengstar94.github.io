---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1498. Number of Subsequences That Satisfy the Given Sum Condition"
date: "2025-02-27"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersInward" 
---


- You are given an array of integers `nums` and an integer `target`.
- Return *the number of **non-empty** subsequences of* `nums` *such that the sum of the minimum and maximum element on it is less or equal to* `target`. Since the answer may be too large, return it **modulo** `10^9 + 7`.

**Example 1**

```
Input: nums = [3,5,6,7], target = 9
Output: 4
Explanation: There are 4 subsequences that satisfy the condition.
[3] -> Min value + max value <= target (3 + 3 <= 9)
[3,5] -> (3 + 5 <= 9)
[3,5,6] -> (3 + 6 <= 9)
[3,6] -> (3 + 6 <= 9)
```

**Example 2**

```
Input: nums = [3,3,6,8], target = 10
Output: 6
Explanation: There are 6 subsequences that satisfy the condition. (nums can have repeated numbers).
[3] , [3] , [3,3], [3,6] , [3,6] , [3,3,6]
```

**Example 3**

```
Input: nums = [2,3,3,4,6,7], target = 12
Output: 61
Explanation: There are 63 non-empty subsequences, two of them do not satisfy the condition ([6,7], [7]).
Number of valid subsequences (63 - 2 = 61).
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/27
 */
public class NumberOfSubsequencesThatSatisfyTheGivenSumCondition {
    public static int numSubseq(int[] nums, int target) {
        // Define the modulus to handle large numbers
        // We use this to prevent integer overflow as per problem requirements
        final int MOD = 1_000_000_007;

        // Sort the array to use two pointer technique
        // After sorting, left pointer will always point to minimum value
        // and right pointer will always point to maximum value in any subsequence
        Arrays.sort(nums);
        int n = nums.length;

        // Pre-calculate powers of 2 to optimize performance
        // pow[i] will store (2^i) % MOD
        // This array helps us calculate the number of possible subsequences quickly
        int[] pow = new int[n];
        pow[0] = 1;  // 2^0 = 1
        for (int i = 1; i < n; i++) {
            // For each index i, calculate 2^i by multiplying previous value by 2
            // Take modulus to prevent overflow
            // Example: pow[3] = 8 means there are 8 possible combinations for 3 numbers
            pow[i] = (pow[i - 1] * 2) % MOD;
        }

        // Initialize two pointers and result
        // left points to potential minimum values
        // right points to potential maximum values
        int left = 0, right = n - 1;
        int result = 0;

        // Process all possible valid subsequences
        while (left <= right) {
            // Check if current min and max sum is within target
            if (nums[left] + nums[right] <= target) {
                // If condition is satisfied:
                // 1. nums[left] will be the minimum value
                // 2. nums[right] will be the maximum value
                // 3. For numbers between left and right (exclusive):
                //    - Each number can either be included or not
                //    - If there are k numbers between left and right
                //    - Total combinations will be 2^k
                //    - We get this value from pow[right-left]
                // Example: for array [2,3,3,4,6,7] when left=0(value=2) and right=5(value=7)
                // - There are 4 numbers between them (3,3,4,6)
                // - Each of these 4 numbers can be included or not
                // - Total combinations = 2^4 = 16
                result = (result + pow[right - left]) % MOD;
                left++;
            } else {
                // If sum is too large, move right pointer to decrease max value
                right--;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Expected output is 4
        // Valid subsequences are: [3], [3,5], [3,5,6], [3,6]
        int[] nums1 = {3, 5, 6, 7};
        int target1 = 9;
        System.out.println("Test Case 1 Result: " + numSubseq(nums1, target1));

        // Test Case 2: Expected output is 6
        // Valid subsequences include duplicates: [3], [3], [3,3], [3,6], [3,6], [3,3,6]
        int[] nums2 = {3, 3, 6, 8};
        int target2 = 10;
        System.out.println("Test Case 2 Result: " + numSubseq(nums2, target2));

        // Test Case 3: Expected output is 61
        // This case demonstrates handling of larger numbers and multiple combinations
        int[] nums3 = {2, 3, 3, 4, 6, 7};
        int target3 = 12;
        System.out.println("Test Case 3 Result: " + numSubseq(nums3, target3));
    }
}

```





