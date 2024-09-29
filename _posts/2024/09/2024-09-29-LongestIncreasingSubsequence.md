---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "300.Longest Increasing Subsequence"
date: "2024-09-29"
categories:
  - "LeetCode Dynamic Programming"
---

- Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.
- **Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1**


- Given an integer array `nums`, return *the length of the longest **strictly increasing*** ***subsequence***.

**Example 1**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2**

```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/09/29
 */
public class LongestIncreasingSubsequence {
    public static int lengthOfLIS(int[] nums) {
        // Edge case: if the input array is null or empty, return 0
        if(nums == null || nums.length == 0){
            return 0;
        }

        int n = nums.length;
        // dp array to store the smallest tail of all increasing subsequences
        // of lengths i+1 in dp[i]
        int[] dp = new int[n];
        // len keeps track of the current length of the longest increasing subsequence
        int len = 0;

        for (int num: nums) {
            // Use binary search to find the position where num should be placed in dp
            // This is more efficient than linear search, reducing time complexity to O(log n)
            int i = Arrays.binarySearch(dp, 0, len, num);

            // If the element is not found, binarySearch returns (-(insertion point) - 1)
            // We need to convert this to the actual insertion point
            if(i < 0){
                i = -(i + 1);
            }

            // Place the current number in its correct position in dp
            // This either replaces a larger element or extends the sequence
            dp[i] = num;

            // If i equals len, it means we've found a new largest element
            // This extends our longest increasing subsequence, so we increment len
            if(i == len){
                len++;
            }
            // Note: If i < len, we're just replacing an existing element
            // This doesn't increase the length, but might help form a longer sequence later
        }

        // The final value of len is the length of the longest increasing subsequence
        return len;
    }

    public static void main(String[] args) {
        int[] nums = {10,9,2,5,3,7,101,18};
        System.out.println("Length of Longest Increasing Subsequence: " + lengthOfLIS(nums));
    }
}

```





