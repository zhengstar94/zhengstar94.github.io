---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3152. Special Array II"
date: "2024-12-09"
tags: Medium
categories:
  - "LeetCode Array"
---

- An array is considered **special** if every pair of its adjacent elements contains two numbers with different parity.
- You are given an array of integer `nums` and a 2D integer matrix `queries`, where for `queries[i] = [fromi, toi]` your task is to check that subarray `nums[fromi..toi]` is **special** or not.
- Return an array of booleans `answer` such that `answer[i]` is `true` if `nums[fromi..toi]` is special.

**Example 1**

```
Input: nums = [3,4,1,2,6], queries = [ [0,4] ]
Output: [false]

Explanation:
The subarray is [3,4,1,2,6]. 2 and 6 are both even.
```

**Example 2**

```
Input: nums = [4,3,1,6], queries = [ [0,2],[2,3] ]
Output: [false,true]

Explanation:
1. The subarray is [4,3,1]. 3 and 1 are both odd. So the answer to this query is false.
2. The subarray is [1,6]. There is only one pair: (1,6) and it contains numbers with different parity. So the answer to this query is true.
```

## Method 1

```tex
【O(n + q) time | O(n) space】
```

```java
package Array;

/**
 * @author zhengxingxing
 * @date 2024/12/09
 */
public class SpecialArrayII {
    public static boolean[] isArraySpecial(int[] nums, int[][] queries) {
        int n = nums.length;
        int m = queries.length;

        // Prefix sum array to track consecutive same parity elements
        int[] preSum = new int[n];

        // Calculate prefix sum by checking parity of consecutive elements
        for (int i = 1; i < n; i++) {
            // If two consecutive elements have the same parity (both odd or both even),
            // increment the prefix sum to mark a break in the "special array" condition
            // If elements have different parity, keep the prefix sum unchanged
            preSum[i] = preSum[i - 1] + ((nums[i - 1] % 2) == (nums[i] % 2) ? 1 : 0);
        }

        // Result array to store query results
        boolean[] result = new boolean[m];

        // Process each query to check if the subarray is special
        for (int i = 0; i < m; i++) {
            // If prefix sums at start and end of query range are equal,
            // it means no consecutive same-parity elements exist in the subarray
            result[i] = (preSum[queries[i][1]] == preSum[queries[i][0]]);
        }

        return result;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {4, 3, 1, 6};
        int[][] queries1 = { { 0, 2}, {2, 3 } };
        boolean[] result1 = isArraySpecial(nums1, queries1);
        System.out.println("Test Case 1 Result:");
        printBooleanArray(result1);  // Expected output: [false, true]

        // Test case 2
        int[] nums2 = {3, 4, 1, 2, 6};
        int[][] queries2 = { { 0, 4 } };
        boolean[] result2 = isArraySpecial(nums2, queries2);
        System.out.println("\nTest Case 2 Result:");
        printBooleanArray(result2);  // Expected output: [false]

        // Test case 3: More complex example
        int[] nums3 = {1, 2, 3, 4, 5, 6, 7, 8};
        int[][] queries3 = { { 0, 3}, {2, 5}, {4, 7 } };
        boolean[] result3 = isArraySpecial(nums3, queries3);
        System.out.println("\nTest Case 3 Result:");
        printBooleanArray(result3);  // Expected output: [true, true, true]
    }

   
    private static void printBooleanArray(boolean[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
    }
}

```





