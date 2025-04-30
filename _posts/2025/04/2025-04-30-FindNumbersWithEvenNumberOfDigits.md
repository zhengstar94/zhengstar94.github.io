---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1295. Find Numbers with Even Number of Digits"
date: "2025-04-30"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given an array `nums` of integers, return how many of them contain an **even number** of digits.

**Example 1**

```
Input: nums = [12,345,2,6,7896]
Output: 2
Explanation: 
12 contains 2 digits (even number of digits). 
345 contains 3 digits (odd number of digits). 
2 contains 1 digit (odd number of digits). 
6 contains 1 digit (odd number of digits). 
7896 contains 4 digits (even number of digits). 
Therefore only 12 and 7896 contain an even number of digits.
```

**Example 2**

```
Input: nums = [555,901,482,1771]
Output: 1 
Explanation: 
Only 1771 contains an even number of digits.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/04/30
 */
public class FindNumbersWithEvenNumberOfDigits {

    public static int findNumbers(int[] nums) {
        // Counter to keep track of numbers with even digits
        int count = 0;

        // Iterate through each number in array
        for(int num : nums) {
            // Convert number to string and check if length is even
            // If even, increment counter
            if(String.valueOf(num).length() % 2 == 0) {
                count++;
            }
        }
        return count;
    }

    public static void main(String[] args) {
        // Test Case 1 demonstration
        int[] nums1 = {12,345,2,6,7896};
        System.out.println("Test Case 1 Result: " + findNumbers(nums1)); // Expected output: 2

        // Test Case 2 demonstration
        int[] nums2 = {555,901,482,1771};
        System.out.println("Test Case 2 Result: " + findNumbers(nums2)); // Expected output: 1
    }
}

```





