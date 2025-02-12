---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "LCP 68. Beautiful Bouquet"
date: "2025-02-12"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindowCountShortest"
---

- At the LeetCode Carnival's flower shop, flowers are arranged in a row from left to right, recorded in an integer array `flowers` where each number represents the variety ID of the flower at that position. You can select a continuous segment of flowers to make a floral arrangement, and no flowers can be discarded. In your chosen arrangement, if the quantity of each flower variety does not exceed `cnt` flowers, then we consider this floral arrangement to be "aesthetically pleasing."

  > For example: In `[5,5,5,6,6]`, there are `3` flowers of variety `5` and `2` flowers of variety `6`. The quantity of **each variety** does not exceed `3`.

- Return the total number of possible continuous segments that can be selected from this row of flowers where the resulting floral arrangement would be "aesthetically pleasing."

- Note:

  - The answer should be calculated modulo `1e9 + 7 (1000000007)`. For example, if the initial result is `1000000008`, return `1`.

**Example 1**

```
Let me translate this example with its explanation:
Input: flowers = [1,2,3,2], cnt = 1
Output: 8
Explanation: For arrangements where each flower variety cannot exceed 1 flower, there are 8 aesthetically pleasing arrangements:
Length-1 segments: [1], [2], [3], [2] all meet the condition, giving 4 possible segments
Length-2 segments: [1,2], [2,3], [3,2] all meet the condition, giving 3 possible segments
Length-3 segments: [1,2,3] meets the condition, giving 1 possible segment
Segments [2,3,2] and [1,2,3,2] both contain 2 flowers of variety 2, so they don't meet the condition.
Total count is 4+3+1 = 8
```

**Example 2**

```
Input: flowers = [5,3,3,3], cnt = 2
Output: 8
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountShortest;

/**
 * @author zhengxingxing
 * @date 2025/02/12
 */
public class BeautifulBouquet {
    // Constant for modulo operation to prevent integer overflow
    private static final int MOD = 1000000007;
    
    public static int beautifulBouquet(int[] flowers, int cnt) {
        // Array to store the count of each flower type
        // Size is 100001 because flower types range from 1 to 100000
        int[] count = new int[100001];

        // Use long to prevent overflow during calculations
        long result = 0;

        // Left pointer of sliding window
        int left = 0;

        // Iterate through the array using right pointer
        for (int right = 0; right < flowers.length; right++) {
            // Increment count of current flower type at right pointer
            count[flowers[right]]++;

            // Shrink window from left while current flower type exceeds cnt
            // This ensures window always contains valid counts
            while (left <= right && count[flowers[right]] > cnt) {
                count[flowers[left]]--;
                left++;
            }

            // Calculate number of valid subarrays ending at current right pointer
            // For current window [left, right]:
            // - We can form subarrays: [right], [right-1,right], [right-2,right],...,[left,right]
            // - Total count of such subarrays is (right - left + 1)
            result = (result + (right - left + 1)) % MOD;
        }

        return (int)result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with repeated flower
        int[] flowers1 = {1, 2, 3, 2};
        int cnt1 = 1;
        System.out.println("Test Case 1 Result: " + beautifulBouquet(flowers1, cnt1));
        // Expected output: 8

        // Test Case 2: Case with multiple repeated flowers
        int[] flowers2 = {5, 3, 3, 3};
        int cnt2 = 2;
        System.out.println("Test Case 2 Result: " + beautifulBouquet(flowers2, cnt2));
        // Expected output: 8

        // Test Case 3: Edge case with all same flowers
        int[] flowers3 = {1, 1, 1, 1};
        int cnt3 = 2;
        System.out.println("Test Case 3 Result: " + beautifulBouquet(flowers3, cnt3));
        // Testing case with all identical elements

        // Test Case 4: Edge case with single element
        int[] flowers4 = {1};
        int cnt4 = 1;
        System.out.println("Test Case 4 Result: " + beautifulBouquet(flowers4, cnt4));
        // Testing single element case
    }
}

```





