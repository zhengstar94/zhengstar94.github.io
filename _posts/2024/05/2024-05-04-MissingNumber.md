---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "268.Missing Number"
date: "2024-05-04"
categories:
  - "LeetCode BitManipulation"
---

# 268. Missing Number

- Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return *the only number in the range that is missing from the array.*

**Example 1**

```
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 2**

```
Input: nums = [0,1]
Output: 2
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 3**

```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.BitManipulation;

/**
 * @author zhengstars
 * @date 2024/06/11
 */
public class MissingNumber {
    public static int findMissingNumber(int[] nums) {
        int n = nums.length;

        // Calculate the expected sum of numbers from 0 to n using the formula
        int totalSum = n * (n + 1) / 2;

        // Calculate the sum of elements in the given array
        int arraySum = 0;
        for (int num : nums) {
            arraySum += num;
        }

        // The missing number is the difference between the totalSum and the arraySum
        return totalSum - arraySum;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {3, 0, 1};
        // Expected output: 2
        System.out.println("Missing number: " + findMissingNumber(nums1));

        // Test case 2
        int[] nums2 = {0, 1};
        // Expected output: 2
        System.out.println("Missing number: " + findMissingNumber(nums2));

        // Test case 3
        int[] nums3 = {9, 6, 4, 2, 3, 5, 7, 0, 1};
        // Expected output: 8
        System.out.println("Missing number: " + findMissingNumber(nums3));
    }
}

```

