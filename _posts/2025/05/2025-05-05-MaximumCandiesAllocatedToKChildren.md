---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2226. Maximum Candies Allocated to K Children"
date: "2025-05-05"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.FindMaximum"
---

# 

- You are given a **0-indexed** integer array `candies`. Each element in the array denotes a pile of candies of size `candies[i]`. You can divide each pile into any number of **sub piles**, but you **cannot** merge two piles together.
- You are also given an integer `k`. You should allocate piles of candies to `k` children such that each child gets the **same** number of candies. Each child can be allocated candies from **only one** pile of candies and some piles of candies may go unused.
- Return *the **maximum number of candies** each child can get.*

**Example 1**

```
Input: candies = [5,8,6], k = 3
Output: 5
Explanation: We can divide candies[1] into 2 piles of size 5 and 3, and candies[2] into 2 piles of size 5 and 1. We now have five piles of candies of sizes 5, 5, 3, 5, and 1. We can allocate the 3 piles of size 5 to 3 children. It can be proven that each child cannot receive more than 5 candies.
```

**Example 2**

```
Input: candies = [2,5], k = 11
Output: 0
Explanation: There are 11 children but only 7 candies in total, so it is impossible to ensure each child receives at least one candy. Thus, each child gets no candy and the answer is 0.
```

## Method 1

```tex
【O(nlogU) time | O(1) space】
```

```java
package Leetcode.BinarySearch.FindMaximum;

/**
 * @author zhengxingxing
 * @date 2025/05/05
 */
public class MaximumCandiesAllocatedToKChildren {

    public static int maximumCandies(int[] candies, long k) {
        // Initialize variables to track maximum pile size and total candies
        int mx = 0;  // Maximum pile size
        long sum = 0;  // Total number of candies

        // Calculate the maximum pile size and sum of all candies
        for (int c : candies) {
            mx = Math.max(mx, c);
            sum += c;
        }

        // Set the boundaries for binary search
        // The minimum candies per child is 0
        int left = 0;
        // The maximum candies per child can't exceed the maximum pile size
        // or the average candies per child (sum/k)
        // We add 1 to make it an exclusive upper bound
        int right = (int) Math.min(mx, sum / k) + 1;

        // Binary search to find the maximum number of candies
        // The search continues until left and right are adjacent
        while (left + 1 < right) {
            // Calculate the middle point to check
            // This formula prevents integer overflow
            int mid = left + (right - left) / 2;

            // If we can distribute 'mid' candies to each child
            if (check(mid, candies, k)) {
                // Try a larger amount
                left = mid;
            } else {
                // Try a smaller amount
                right = mid;
            }
        }

        // Return the maximum valid amount
        return left;
    }
    
    private static boolean check(int low, int[] candies, long k) {
        // Handle edge case where low is 0
        if (low == 0) {
            return k == 0;  // Only true if there are no children
        }

        // Count how many children can receive 'low' candies
        long sum = 0;
        for (int c : candies) {
            // Each pile can serve c/low children
            sum += c / low;

            // Early termination if we've already satisfied all children
            if (sum >= k) {
                return true;
            }
        }

        // Return true if we can serve at least k children
        return sum >= k;
    }
    
    public static void main(String[] args) {
        // Test case 1
        int[] candies1 = {5, 8, 6};
        int k1 = 3;
        System.out.println("Test Case 1:");
        System.out.println("Input: candies = [5, 8, 6], k = 3");
        System.out.println("Output: " + maximumCandies(candies1, k1));
        System.out.println("Expected: 5");
        System.out.println();

        // Test case 2
        int[] candies2 = {2, 5};
        int k2 = 11;
        System.out.println("Test Case 2:");
        System.out.println("Input: candies = [2, 5], k = 11");
        System.out.println("Output: " + maximumCandies(candies2, k2));
        System.out.println("Expected: 0");
        System.out.println();
    }
}
```





