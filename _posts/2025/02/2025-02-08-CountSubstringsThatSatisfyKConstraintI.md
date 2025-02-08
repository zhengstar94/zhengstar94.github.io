---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3258. Count Substrings That Satisfy K-Constraint I"
date: "2025-02-08"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountShortest"
---


- You are given a **binary** string `s` and an integer `k`.
- A **binary string** satisfies the **k-constraint** if **either** of the following conditions holds:
  - The number of `0`'s in the string is at most `k`.
  - The number of `1`'s in the string is at most `k`.
- Return an integer denoting the number of substrings of `s` that satisfy the **k-constraint**.

**Example 1**

```
Input: s = "10101", k = 1

Output: 12

Explanation:

Every substring of s except  the substrings "1010", "10101", and "0101" satisfies the k-constraint.
```

**Example 2**

```
Input: s = "1010101", k = 2

Output: 25

Explanation:

Every substring of s except  the substrings with a length greater than 5 satisfies the k-constraint.
```

**Example 3**

```
Input: s = "11111", k = 1

Output: 15

Explanation:

All substrings of s satisfy the k-constraint.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountShortest;

/**
 * @author zhengxingxing
 * @date 2025/02/08
 */
public class CountSubstringsThatSatisfyKConstraintI {

    public static int countKConstraintSubstrings(String s, int k) {
        int cnt0 = 0; // Keeps track of the count of '0's in the current window
        int cnt1 = 0; // Keeps track of the count of '1's in the current window
        int left = 0; // The left boundary of the dynamic sliding window
        int result = 0; // Stores the total count of valid substrings

        // Iterate over the string with the right boundary expanding
        for (int right = 0; right < s.length(); right++) {
            // Expand the window by including the current character `s.charAt(right)`
            if (s.charAt(right) == '0') {
                cnt0++;  // Increment the count of '0's
            } else {
                cnt1++;  // Increment the count of '1's
            }

            // Shrink the window (move the left boundary) as long as it violates the k-constraint:
            // Both cnt0 and cnt1 cannot exceed `k`.
            while (cnt0 > k && cnt1 > k) {
                // Remove the character at the left boundary from the window
                if (s.charAt(left) == '0') {
                    cnt0--;  // Decrement the count of '0's
                } else {
                    cnt1--;  // Decrement the count of '1's
                }
                left++; // Move the left boundary one step forward
            }

            // Add the number of valid substrings in the current window:
            // The number of substrings ending at `right` is equal to the size of the window.
            result += right - left + 1;
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test case 1
        String s1 = "10101";
        int k1 = 1;
        System.out.println("Result for test case 1: " + countKConstraintSubstrings(s1, k1));
        // Output: 12

        // Test case 2
        String s2 = "1010101";
        int k2 = 2;
        System.out.println("Result for test case 2: " + countKConstraintSubstrings(s2, k2));
        // Output: 25

        // Test case 3
        String s3 = "11111";
        int k3 = 1;
        System.out.println("Result for test case 3: " + countKConstraintSubstrings(s3, k3));
        // Output: 15
    }
}
```





