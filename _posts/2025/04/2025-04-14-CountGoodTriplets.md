---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1534. Count Good Triplets"
date: "2025-04-14"
tags: Easy
categories:
  - "LeetCode BruteForce"
---


- Given an array of integers `arr`, and three integers `a`, `b` and `c`. You need to find the number of good triplets.
- A triplet `(arr[i], arr[j], arr[k])` is **good** if the following conditions are true:
  - `0 <= i < j < k < arr.length`
  - `|arr[i] - arr[j]| <= a`
  - `|arr[j] - arr[k]| <= b`
  - `|arr[i] - arr[k]| <= c`
- Where `|x|` denotes the absolute value of `x`.
- Return *the number of good triplets*.

**Example 1**

```
Input: arr = [3,0,1,1,9,7], a = 7, b = 2, c = 3
Output: 4
Explanation: There are 4 good triplets: [(3,0,1), (3,0,1), (3,1,1), (0,1,1)].
```

**Example 2**

```
Input: arr = [1,1,2,2,3], a = 0, b = 0, c = 1
Output: 0
Explanation: No triplet satisfies all conditions.
```

## Method 1

```tex
【O(n^3) time | O(1) space】
```

```java
package Leetcode.BruteForce;

/**
 * @author zhengxingxing
 * @date 2025/04/14
 */
public class CountGoodTriplets {

    public static int countGoodTriplets(int[] arr, int a, int b, int c) {
        // Counter to keep track of valid triplets
        int count = 0;
        // Length of input array
        int n = arr.length;

        // First loop for index i
        for (int i = 0; i < n - 2; i++) {
            // Second loop for index j, starting after i
            for (int j = i + 1; j < n - 1; j++) {
                // Early check for first condition to avoid unnecessary iterations
                if (Math.abs(arr[i] - arr[j]) > a){
                    continue;
                }

                // Third loop for index k, starting after j
                for (int k = j + 1; k < n; k++) {
                    // Check if triplet satisfies all remaining conditions
                    if (Math.abs(arr[j] - arr[k]) <= b && Math.abs(arr[i] - arr[k]) <= c){
                        count++;
                    }
                }
            }
        }

        return count;
    }


    public static void main(String[] args) {
        // Test Case 1: Expected output is 4
        // Example where multiple good triplets exist
        int[] arr1 = {3,0,1,1,9,7};
        int a1 = 7, b1 = 2, c1 = 3;
        System.out.println("Test Case 1 Result: " + countGoodTriplets(arr1, a1, b1, c1));

        // Test Case 2: Expected output is 0
        // Example where no good triplets exist
        int[] arr2 = {1,1,2,2,3};
        int a2 = 0, b2 = 0, c2 = 1;
        System.out.println("Test Case 2 Result: " + countGoodTriplets(arr2, a2, b2, c2));
    }
}

```





