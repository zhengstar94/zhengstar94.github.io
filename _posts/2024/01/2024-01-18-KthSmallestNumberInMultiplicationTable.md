---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "668. Kth Smallest Number in Multiplication Table"
date: "2024-01-18"
categories:
  - "LeetCode Binary Search"
---

# LeetCode 668. Kth Smallest Number in Multiplication Table [Hard]

- Nearly every one have used the [Multiplication Table](https://en.wikipedia.org/wiki/Multiplication_table). But could you find out the `k-th` smallest number quickly from the multiplication table?
- Given the height `m` and the length `n` of a `m * n` Multiplication Table, and a positive integer `k`, you need to return the `k-th` smallest number in this table.

**Example 1**

```
Input: m = 3, n = 3, k = 5
Output: 
Explanation: 
The Multiplication Table:
1	2	3
2	4	6
3	6	9

The 5-th smallest number is 3 (1, 2, 2, 3, 3).
```

**Example 2**

```
Input: m = 2, n = 3, k = 6
Output: 
Explanation: 
The Multiplication Table:
1	2	3
2	4	6

The 6-th smallest number is 6 (1, 2, 2, 3, 4, 6).
```

## 

## Method 1

```tex
【O(mlog(mn))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

import codeTest.test;

/**
 * @author zhengstars
 * @date 2024/02/03
 */
public class KthSmallestNumberInMultiplicationTable {

    public static int findKthNumber(int m, int n, int k) {
        // Define the lowest possible number (1) and the highest possible number (m * n).
        int low = 1;
        int high = m * n;

        // Binary search.
        while (low < high) {
            // Calculate mid number.
            int mid = low + (high - low) / 2;

            // Count the quantity of numbers not larger than mid.
            // The quantity is the accumulation of each row.
            int count = 0;
            int j = n;
            for (int i = 1; i <= m; i++) {
                // Decrease j while the current number (i * j) is larger than mid.
                while (j >= 1 && mid < i * j) {
                    j--;
                }

                // Add the quantity of numbers not larger than mid in the i-th row.
                count += j;
            }

            // If count < k, then mid is too small.
            if (count < k) {
                low = mid + 1;
            }
            // If count >= k, then mid is large enough, but we have to continue to try to find a smaller one that meets the condition.
            else {
                high = mid;
            }
        }

        // Finally, high is the k-th smallest number.
        return high;
    }

    public static void main(String[] args) {
        // Test case 1: The height m=3, length n=3 of a m * n Multiplication Table, with a positive integer k=5
        System.out.println(findKthNumber(3, 3, 5)); // Output: 3

        // Test case 2: The height m=2, length n=3 of a m * n Multiplication Table, with a positive integer k=6
        System.out.println(findKthNumber(2, 3, 6)); // Output: 6
    }
}

```

