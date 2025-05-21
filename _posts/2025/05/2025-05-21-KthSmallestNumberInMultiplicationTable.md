---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "668. Kth Smallest Number in Multiplication Table"
date: "2025-05-21"
tags: Hard BinarySearch
categories:
  - "LeetCode BinarySearch.KthElement"
---


- Nearly everyone has used the [Multiplication Table](https://en.wikipedia.org/wiki/Multiplication_table). The multiplication table of size `m x n` is an integer matrix `mat` where `mat[i][j] == i * j` (**1-indexed**).
- Given three integers `m`, `n`, and `k`, return *the* `kth` *smallest element in the* `m x n` *multiplication table*.

**Example 1**

```
Input: m = 3, n = 3, k = 5
Output: 3
Explanation: The 5th smallest number is 3.
```

**Example 2**

```
Input: m = 2, n = 3, k = 6
Output: 6
Explanation: The 6th smallest number is 6.
```

## Method 1

```tex
【O(mlog(mn)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.KthElement;

/**
 * @author zhengxingxing
 * @date 2025/05/21
 */
public class KthSmallestNumberInMultiplicationTable {

    public static int findKthNumber(int m, int n, int k) {
        // Initialize the binary search boundaries:
        // - left: smallest possible value in the table (always 1)
        // - right: largest possible value in the table (m*n)
        int left = 1;
        int right = m * n;

        // Binary search loop
        while (left < right) {
            // Calculate the middle value to test
            int x = left + (right - left) / 2;

            // Count elements less than or equal to x in the multiplication table
            // First, handle complete rows (rows where all elements are <= x)
            int count = x / n * n;  // (x/n) complete rows, each contributing n elements

            // Then, handle partial rows (rows where only some elements are <= x)
            for (int i = x / n + 1; i <= m; ++i) {
                // In row i, elements less than or equal to x are: i, 2i, 3i, ..., up to x/i multiples
                count += x / i;
            }

            // Adjust search boundaries
            if (count >= k) {
                // If we have at least k elements <= x, the kth smallest could be x or smaller
                right = x;
            } else {
                // If we have fewer than k elements <= x, the kth smallest must be larger than x
                left = x + 1;
            }
        }

        // When left == right, we've found the kth smallest number
        return left;
    }

    /**
     * Main method with test cases
     */
    public static void main(String[] args) {
        // Test Case 1: m=3, n=3, k=5 (Expected output: 3)
        System.out.println("Test Case 1:");
        System.out.println("Input: m=3, n=3, k=5");
        System.out.println("Output: " + findKthNumber(3, 3, 5));
        System.out.println("Expected: 3");
        System.out.println();

        // Test Case 2: m=2, n=3, k=6 (Expected output: 6)
        System.out.println("Test Case 2:");
        System.out.println("Input: m=2, n=3, k=6");
        System.out.println("Output: " + findKthNumber(2, 3, 6));
        System.out.println("Expected: 6");
        System.out.println();

        // Test Case 3: m=5, n=6, k=10 (Expected output: 5)
        System.out.println("Test Case 3:");
        System.out.println("Input: m=5, n=6, k=10");
        System.out.println("Output: " + findKthNumber(5, 6, 10));
        System.out.println("Expected: 5");
        System.out.println();
    }
}
```





