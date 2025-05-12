---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2064. Minimized Maximum of Products Distributed to Any Store"
date: "2025-05-12"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.MinimizeMaximum"
---

- You are given an integer `n` indicating there are `n` specialty retail stores. There are `m` product types of varying amounts, which are given as a **0-indexed** integer array `quantities`, where `quantities[i]` represents the number of products of the `ith` product type.
- You need to distribute **all products** to the retail stores following these rules:
  - A store can only be given **at most one product type** but can be given **any** amount of it.
  - After distribution, each store will have been given some number of products (possibly `0`). Let `x` represent the maximum number of products given to any store. You want `x` to be as small as possible, i.e., you want to **minimize** the **maximum** number of products that are given to any store.
- Return *the minimum possible* `x`.

**Example 1**

```
Input: n = 6, quantities = [11,6]
Output: 3
Explanation: One optimal way is:
- The 11 products of type 0 are distributed to the first four stores in these amounts: 2, 3, 3, 3
- The 6 products of type 1 are distributed to the other two stores in these amounts: 3, 3
The maximum number of products given to any store is max(2, 3, 3, 3, 3, 3) = 3.

```

**Example 2**

```
Input: n = 7, quantities = [15,10,10]
Output: 5
Explanation: One optimal way is:
- The 15 products of type 0 are distributed to the first three stores in these amounts: 5, 5, 5
- The 10 products of type 1 are distributed to the next two stores in these amounts: 5, 5
- The 10 products of type 2 are distributed to the last two stores in these amounts: 5, 5
The maximum number of products given to any store is max(5, 5, 5, 5, 5, 5, 5) = 5.
```

**Example 3**

```
Input: n = 1, quantities = [100000]
Output: 100000
Explanation: The only optimal way is:
- The 100000 products of type 0 are distributed to the only store.
The maximum number of products given to any store is max(100000) = 100000.
```

## Method 1

```tex
【O(k * log(n)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.MinimizeMaximum;

/**
 * @author zhengxingxing
 * @date 2025/05/12
 */
public class MinimizedMaximumOfProductsDistributedToAnyStore {
    
    public static int minimizedMaximum(int n, int[] quantities) {
        int left = 1;  // The minimum possible maxProducts is 1 (each store gets at least 1 product)
        int right = 0; // Will hold the maximum of all quantities to initialize upper bound

        // Find the maximum product quantity among all types to set the upper bound
        for (int quantity : quantities) {
            right = Math.max(right, quantity);
        }

        // Binary search for the smallest feasible maxProducts value
        while (left < right) {
            int mid = left + (right - left) / 2; // Avoids overflow

            // Check if it's possible to distribute all products with maxProducts = mid
            if (canDistribute(n, quantities, mid)) {
                // If it's possible, try a smaller max value to minimize the maximum
                right = mid;
            } else {
                // If not possible, we need to increase the allowed max per store
                left = mid + 1;
            }
        }

        // After the loop, left == right and it's the smallest feasible maxProducts value
        return left;
    }
    
    private static boolean canDistribute(int n, int[] quantities, int maxProducts) {
        int storesNeeded = 0; // Total number of stores needed under current maxProducts constraint

        for (int quantity : quantities) {
            // For each product type, calculate how many stores are needed
            // Formula: ceil(quantity / maxProducts)
            storesNeeded += (quantity + maxProducts - 1) / maxProducts;

            // If more stores are needed than we have, the distribution fails
            if (storesNeeded > n) {
                return false;
            }
        }

        // If we can serve all products within n stores, return true
        return true;
    }

    public static void main(String[] args) {
        // Test Case 1
        int n1 = 6;
        int[] quantities1 = {11, 6};
        System.out.println("Test Case 1 Output: " + minimizedMaximum(n1, quantities1));
        // Expected output: 3

        // Test Case 2
        int n2 = 7;
        int[] quantities2 = {15, 10, 10};
        System.out.println("Test Case 2 Output: " + minimizedMaximum(n2, quantities2));
        // Expected output: 5

        // Test Case 3
        int n3 = 1;
        int[] quantities3 = {100000};
        System.out.println("Test Case 3 Output: " + minimizedMaximum(n3, quantities3));
        // Expected output: 100000
    }
}

```





