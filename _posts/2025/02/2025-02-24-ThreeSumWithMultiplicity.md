---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "923. 3Sum With Multiplicity"
date: "2025-02-24"
tags: Medium
categories:
  - "LeetCode TwoPointers"
---

- Given an integer array `arr`, and an integer `target`, return the number of tuples `i, j, k` such that `i < j < k` and `arr[i] + arr[j] + arr[k] == target`.
- As the answer can be very large, return it **modulo** `109 + 7`.


**Example 1**

```
Input: arr = [1,1,2,2,3,3,4,4,5,5], target = 8
Output: 20
Explanation: 
Enumerating by the values (arr[i], arr[j], arr[k]):
(1, 2, 5) occurs 8 times;
(1, 3, 4) occurs 8 times;
(2, 2, 4) occurs 2 times;
(2, 3, 3) occurs 2 times.
```

**Example 2**

```
Input: arr = [1,1,2,2,2,2], target = 5
Output: 12
Explanation: 
arr[i] = 1, arr[j] = arr[k] = 2 occurs 12 times:
We choose one 1 from [1,1] in 2 ways,
and two 2s from [2,2,2,2] in 6 ways.
```

**Example 3**

```
Input: arr = [2,1,3], target = 6
Output: 1
Explanation: (1, 2, 3) occured one time in the array so we return 1.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/02/24
 */
public class ThreeSumWithMultiplicity {
    // Constant for modulo operation as per problem requirement
    static final int MOD = 1000000007;

    public static int threeSumMulti(int[] arr, int target) {
        // Count array to store frequency of each number
        // Since constraints mention array elements <= 100, we use array size 101
        long[] count = new long[101];
        for (int num : arr) {
            count[num]++;
        }

        long result = 0;

        // Case 1: All three numbers are different (i != j != k)
        // Example: For target = 8, finding combinations like (1,2,5) where:
        // - i will be 1 (appears count[1] times)
        // - j will be 2 (appears count[2] times)
        // - k will be 5 (appears count[5] times)
        // Total combinations = count[1] * count[2] * count[5]
        for (int i = 0; i <= 100; i++) {
            for (int j = i + 1; j <= 100; j++) {
                int k = target - i - j;
                if (k > j && k <= 100) {
                    result += count[i] * count[j] * count[k];
                }
            }
        }

        // Case 2: First two numbers are same (i == j != k)
        // Example: For target = 7, finding combinations like (2,2,3) where:
        // - Choose 2 numbers from count[i] numbers: C(count[i],2) = count[i] * (count[i]-1) / 2
        // - Then multiply by count[k] for the third different number
        for (int i = 0; i <= 100; i++) {
            int k = target - 2 * i;
            if (k > i && k <= 100) {
                result += count[i] * (count[i] - 1) / 2 * count[k];
            }
        }

        // Case 3: Last two numbers are same (i != j == k)
        // Example: For target = 7, finding combinations like (1,3,3) where:
        // - Choose one count[i]
        // - Then choose 2 numbers from count[j] numbers: C(count[j],2)
        for (int i = 0; i <= 100; i++) {
            if ((target - i) % 2 == 0) {
                int j = (target - i) / 2;
                if (j > i && j <= 100) {
                    result += count[i] * count[j] * (count[j] - 1) / 2;
                }
            }
        }

        // Case 4: All three numbers are same (i == j == k)
        // Example: For target = 6, finding combinations like (2,2,2) where:
        // - Choose 3 numbers from count[i] numbers: C(count[i],3)
        // - Formula: n * (n-1) * (n-2) / 6
        if (target % 3 == 0) {
            int i = target / 3;
            if (i <= 100) {
                result += count[i] * (count[i] - 1) * (count[i] - 2) / 6;
            }
        }

        // Return final result with modulo operation
        return (int)(result % MOD);
    }

    public static void main(String[] args) {
        // Test case 1: Multiple combinations of different numbers
        int[] arr1 = {1,1,2,2,3,3,4,4,5,5};
        int target1 = 8;
        System.out.println(threeSumMulti(arr1, target1)); // Expected output: 20

        // Test case 2: Combinations with repeated numbers
        int[] arr2 = {1,1,2,2,2,2};
        int target2 = 5;
        System.out.println(threeSumMulti(arr2, target2)); // Expected output: 12

        // Test case 3: Simple case with three different numbers
        int[] arr3 = {2,1,3};
        int target3 = 6;
        System.out.println(threeSumMulti(arr3, target3)); // Expected output: 1
    }
}

```





