---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2367. Number of Arithmetic Triplets"
date: "2025-04-04"
tags: Medium TwoPointers
categories:
  - "LeetCode ThreePointers"
---

- You are given a **0-indexed**, **strictly increasing** integer array `nums` and a positive integer `diff`. A triplet `(i, j, k)` is an **arithmetic triplet** if the following conditions are met:
  - `i < j < k`,
  - `nums[j] - nums[i] == diff`, and
  - `nums[k] - nums[j] == diff`.
- Return *the number of unique **arithmetic triplets**.*

**Example 1**

```
Input: nums = [0,1,4,6,7,10], diff = 3
Output: 2
Explanation:
(1, 2, 4) is an arithmetic triplet because both 7 - 4 == 3 and 4 - 1 == 3.
(2, 4, 5) is an arithmetic triplet because both 10 - 7 == 3 and 7 - 4 == 3. 
```

**Example 2**

```
Input: nums = [4,5,6,7,8,9], diff = 2
Output: 2
Explanation:
(0, 2, 4) is an arithmetic triplet because both 8 - 6 == 2 and 6 - 4 == 2.
(1, 3, 5) is an arithmetic triplet because both 9 - 7 == 2 and 7 - 5 == 2.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.ThreePointer;

/**
 * @author zhengxingxing
 * @date 2025/04/04
 */
public class NumberOfArithmeticTriplets {

    public static int arithmeticTriplets(int[] nums, int diff) {
        // Counter for arithmetic triplets
        int ans = 0;
        // Pointer i for the first number in the triplet
        int i = 0;
        // Pointer j for the second number in the triplet
        int j = 1;

        // Iterate through the array, treating each element as the third number (x)
        for (int x: nums) {
            // Move pointer j until we find a potential second number
            // We want: nums[j] + diff >= x
            while (nums[j] + diff < x){
                j++;
            }

            // If nums[j] + diff > x, no valid triplet possible with current x
            if (nums[j] + diff > x){
                continue;
            }

            // Move pointer i until we find a potential first number
            // We want: nums[i] + 2*diff >= x
            while (nums[i] + diff * 2 < x){
                i++;
            }

            // If we found a perfect match where nums[i] + 2*diff = x,
            // we've found an arithmetic triplet
            if (nums[i] + diff * 2 == x){
                ans++;
            }
        }

        return ans;
    }


    public static void main(String[] args) {
        // Test Case 1: Multiple arithmetic triplets
        int[] nums1 = {0, 1, 4, 6, 7, 10};
        int diff1 = 3;
        System.out.println("Test Case 1");
        System.out.println("Input: nums = [0,1,4,6,7,10], diff = 3");
        System.out.println("Expected Output: 2");
        System.out.println("Actual Output: " + arithmeticTriplets(nums1, diff1));
        System.out.println();

        // Test Case 2: Consecutive arithmetic triplets
        int[] nums2 = {4, 5, 6, 7, 8, 9};
        int diff2 = 2;
        System.out.println("Test Case 2");
        System.out.println("Input: nums = [4,5,6,7,8,9], diff = 2");
        System.out.println("Expected Output: 2");
        System.out.println("Actual Output: " + arithmeticTriplets(nums2, diff2));
    }
}

```





