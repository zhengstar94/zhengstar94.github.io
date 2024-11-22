---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "525. Contiguous Array"
date: "2024-11-22"
categories:
  - "LeetCode HashTable"
---


- Given a binary array `nums`, return *the maximum length of a contiguous subarray with an equal number of* `0` *and* `1`.

**Example 1**

```
Input: nums = [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.
```

**Example 2**

```
Input: nums = [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/11/22
 */
public class ContiguousArray {
    public static int findMaxLength(int[] nums) {
        // Map to store the first occurrence of each count
        // Key: running sum (count), Value: index where this count first appeared
        // Initialize with 0 at index -1 to handle cases where valid subarray starts from index 0
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);

        // maxLen: tracks the length of the longest valid subarray found so far
        int maxLen = 0;
        // count: running sum (treating 0 as -1 and 1 as 1)
        // When count remains unchanged between two positions, the subarray between them has equal 0s and 1s
        int count = 0;

        for (int i = 0; i < nums.length; i++) {
            // Update running sum: add 1 for 1s, subtract 1 for 0s
            // This helps track the balance between 0s and 1s
            count += nums[i] == 1 ? 1 : -1;

            // If we've seen this count before, we've found a valid subarray
            // Example: if count becomes 0 again, it means we have equal 0s and 1s from the start
            if (map.containsKey(count)) {
                // Calculate length of current valid subarray (current position - previous position with same count)
                // Update maxLen if current valid subarray is longer
                // Example: if count = 1 appears at index 2 and 5, then indexes 3,4,5 form a valid subarray
                maxLen = Math.max(maxLen, i - map.get(count));
            } else {
                // If this is the first time we see this count, record its position
                // We only store the first occurrence to find the longest possible subarray
                map.put(count, i);
            }
        }

        return maxLen;
    }

    public static void main(String[] args) {
        // Test case 1: Basic case with alternating values
        int[] test1 = {0, 1, 0};
        System.out.println("Test case 1: " + findMaxLength(test1)); // Expected output: 2

        // Test case 2: Minimal case
        int[] test2 = {0, 1};
        System.out.println("Test case 2: " + findMaxLength(test2)); // Expected output: 2

        // Test case 3: Grouped zeros and ones
        int[] test3 = {0, 0, 1, 1};
        System.out.println("Test case 3: " + findMaxLength(test3)); // Expected output: 4
    }
}

```