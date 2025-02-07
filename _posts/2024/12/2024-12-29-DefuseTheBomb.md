---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1652. Defuse the Bomb"
date: "2024-12-29"
tags: Easy SlideWindow
categories:
  - "LeetCode SlideWindow"
---


- You have a bomb to defuse, and your time is running out! Your informer will provide you with a **circular** array `code` of length of `n` and a key `k`.
- To decrypt the code, you must replace every number. All the numbers are replaced **simultaneously**.
  - If `k > 0`, replace the `ith` number with the sum of the **next** `k` numbers.
  - If `k < 0`, replace the `ith` number with the sum of the **previous** `k` numbers.
  - If `k == 0`, replace the `ith` number with `0`.
- As `code` is circular, the next element of `code[n-1]` is `code[0]`, and the previous element of `code[0]` is `code[n-1]`.
- Given the **circular** array `code` and an integer key `k`, return *the decrypted code to defuse the bomb*!

**Example 1**

```
Input: code = [5,7,1,4], k = 3
Output: [12,10,16,13]
Explanation: Each number is replaced by the sum of the next 3 numbers. The decrypted code is [7+1+4, 1+4+5, 4+5+7, 5+7+1]. Notice that the numbers wrap around.
```

**Example 2**

```
Input: code = [1,2,3,4], k = 0
Output: [0,0,0,0]
Explanation: When k is zero, the numbers are replaced by 0. 
```

**Example 3**

```
Input: code = [2,4,9,3], k = -2
Output: [12,5,6,13]
Explanation: The decrypted code is [3+9, 2+3, 4+2, 9+4]. Notice that the numbers wrap around again. If k is negative, the sum is of the previous numbers.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/29
 */
public class DefuseTheBomb {
    public static int[] decrypt(int[] code, int k) {
        int n = code.length;
        int[] ans = new int[n];  // Store the result array

        // Initialize the right boundary 'r'
        // If k > 0: r = k + 1 (points to the position after the first window)
        // If k < 0: r = n (points to the end of array)
        int r = k > 0 ? k + 1 : n;

        // Take absolute value of k to handle both positive and negative cases uniformly
        k = Math.abs(k);

        // Initialize sum for first position
        int s = 0;

        // First loop: Calculate sum for the initial window
        for (int i = r - k; i < r; i++) {
            // For k > 0: directly sum the next k numbers
            // For k < 0: using modulo to wrap around and sum the previous k numbers
            // Example for k = 3: sums code[1] + code[2] + code[3]
            // Example for k = -3: sums code[3] + code[4] + code[5] (using i % n)
            s += code[i % n];
        }

        // Second loop: Slide the window to calculate sums for all positions
        for (int i = 0; i < n; i++) {
            // Store current window sum in result array
            ans[i] = s;

            // Slide window by:
            // 1. Adding new element (code[r % n])
            // 2. Removing leftmost element (code[(r - k) % n])
            // Example: if current sum is [1,4,1], next sum will be [4,1,5]
            // by adding code[4]=5 and removing code[1]=1
            s += code[r % n] - code[(r - k) % n];
            r++;  // Move window right by one position
        }

        return ans;
    }

    /**
     * Main method to test the solution with example cases
     */
    public static void main(String[] args) {
        // Test Case 1: k > 0
        int[] code1 = {5, 7, 1, 4};
        int k1 = 3;
        int[] result1 = decrypt(code1, k1);
        System.out.println("Test Case 1 (k > 0):");
        System.out.println("Input: code = [5,7,1,4], k = 3");
        System.out.print("Output: [");
        for (int i = 0; i < result1.length; i++) {
            System.out.print(result1[i] + (i < result1.length - 1 ? "," : ""));
        }
        System.out.println("]");  // Expected: [12,10,16,13]

        // Test Case 2: k = 0
        int[] code2 = {1, 2, 3, 4};
        int k2 = 0;
        int[] result2 = decrypt(code2, k2);
        System.out.println("\nTest Case 2 (k = 0):");
        System.out.println("Input: code = [1,2,3,4], k = 0");
        System.out.print("Output: [");
        for (int i = 0; i < result2.length; i++) {
            System.out.print(result2[i] + (i < result2.length - 1 ? "," : ""));
        }
        System.out.println("]");  // Expected: [0,0,0,0]

        // Test Case 3: k < 0
        int[] code3 = {2, 4, 9, 3};
        int k3 = -2;
        int[] result3 = decrypt(code3, k3);
        System.out.println("\nTest Case 3 (k < 0):");
        System.out.println("Input: code = [2,4,9,3], k = -2");
        System.out.print("Output: [");
        for (int i = 0; i < result3.length; i++) {
            System.out.print(result3[i] + (i < result3.length - 1 ? "," : ""));
        }
        System.out.println("]");  // Expected: [12,5,6,13]
    }
}

```





