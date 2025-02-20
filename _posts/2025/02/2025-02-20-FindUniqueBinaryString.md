---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1980. Find Unique Binary String"
date: "2025-02-20"
tags: Medium
categories:
  - "LeetCode String"
---


- Given an array of strings `nums` containing `n` **unique** binary strings each of length `n`, return *a binary string of length* `n` *that **does not appear** in* `nums`*. If there are multiple answers, you may return **any** of them*.

**Example 1**

```
Input: nums = ["01","10"]
Output: "11"
Explanation: "11" does not appear in nums. "00" would also be correct.
```

**Example 2**

```
Input: nums = ["00","01"]
Output: "11"
Explanation: "11" does not appear in nums. "10" would also be correct.
```

**Example 3**

```
Input: nums = ["111","011","001"]
Output: "101"
Explanation: "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/02/20
 */
public class FindUniqueBinaryString {
    
    public static String findDifferentBinaryString(String[] nums) {
        StringBuilder sb = new StringBuilder();

        // Iterate through each string's diagonal position (i,i)
        for (int i = 0; i < nums.length; ++i) {
            // Flip the bit at diagonal position:
            // If current bit is '0', append '1'
            // If current bit is '1', append '0'
            sb.append(nums[i].charAt(i) == '0' ? '1' : '0');
        }
        return sb.toString();
    }


    public static void main(String[] args) {
        // Test Case 1: Binary strings of length 2
        String[] nums1 = {"01", "10"};
        System.out.println("Test Case 1 Result: " + findDifferentBinaryString(nums1)); // Expected output: "11"

        // Test Case 2: Another set of binary strings of length 2
        String[] nums2 = {"00", "01"};
        System.out.println("Test Case 2 Result: " + findDifferentBinaryString(nums2)); // Expected output: "11"

        // Test Case 3: Binary strings of length 3
        String[] nums3 = {"111", "011", "001"};
        System.out.println("Test Case 3 Result: " + findDifferentBinaryString(nums3)); // Expected output: "101"
    }
}

```





