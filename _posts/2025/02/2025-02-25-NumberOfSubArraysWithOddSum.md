---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1524. Number of Sub-arrays With Odd Sum"
date: "2025-02-25"
tags: Medium
categories:
  - "LeetCode Math"
---

- Given an array of integers `arr`, return *the number of subarrays with an **odd** sum*.
- Since the answer can be very large, return it modulo `10^9 + 7`.


**Example 1**

```
Input: arr = [1,3,5]
Output: 4
Explanation: All subarrays are [ [ 1],[1,3],[1,3,5],[3],[3,5],[5 ] ]
All sub-arrays sum are [1,4,9,3,8,5].
Odd sums are [1,9,3,5] so the answer is 4.

```

**Example 2**

```
Input: arr = [2,4,6]
Output: 0
Explanation: All subarrays are [ [ 2],[2,4],[2,4,6],[4],[4,6],[6 ] ]
All sub-arrays sum are [2,6,12,4,10,6].
All sub-arrays have even sum and the answer is 0.
```

**Example 3**

```
Input: arr = [1,2,3,4,5,6,7]
Output: 16
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/02/25
 */
public class NumberOfSubArraysWithOddSum {
    
    public static int numOfSubarrays(int[] arr) {
        // Define the modulo constant to handle large numbers
        final int MODULO = 1000000007;

        // odd: count of prefix sums that are odd
        // even: count of prefix sums that are even, initialized to 1 (empty array sum is 0, which is even)
        int odd = 0;
        int even = 1;

        // Variable to store the total count of subarrays with odd sum
        int subarrays = 0;
        // Variable to store the running prefix sum
        int sum = 0;

        // Iterate through each element in the array
        for (int i = 0; i < arr.length; i++) {
            // Add current element to prefix sum
            sum += arr[i];

            /* Key Logic:
             * 1. If current prefix sum is even:
             *    - To get odd sum subarray, we need previous odd prefix sums
             *    - Because even - odd = odd
             * 2. If current prefix sum is odd:
             *    - To get odd sum subarray, we need previous even prefix sums
             *    - Because odd - even = odd
             */
            subarrays = (subarrays + (sum % 2 == 0 ? odd : even)) % MODULO;

            // Update the counts of odd and even prefix sums
            if (sum % 2 == 0) {
                // If current prefix sum is even, increment even count
                even++;
            } else {
                // If current prefix sum is odd, increment odd count
                odd++;
            }
        }

        return subarrays;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Expected output is 4
        // Subarrays with odd sum: [1], [1,3,5], [3], [5]
        int[] arr1 = {1, 3, 5};
        System.out.println("Test Case 1 Result: " + numOfSubarrays(arr1));

        // Test Case 2: Expected output is 0
        // All subarrays have even sum
        int[] arr2 = {2, 4, 6};
        System.out.println("Test Case 2 Result: " + numOfSubarrays(arr2));

        // Test Case 3: Expected output is 16
        int[] arr3 = {1, 2, 3, 4, 5, 6, 7};
        System.out.println("Test Case 3 Result: " + numOfSubarrays(arr3));
    }
}

```





