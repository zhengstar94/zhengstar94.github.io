---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2517. Maximum Tastiness of Candy Basket"
date: "2025-05-18"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.MaximizeMinimum"
---

- You are given an array of positive integers `price` where `price[i]` denotes the price of the `ith` candy and a positive integer `k`.
- The store sells baskets of `k` **distinct** candies. The **tastiness** of a candy basket is the smallest absolute difference of the **prices** of any two candies in the basket.
- Return *the **maximum** tastiness of a candy basket.*

**Example 1**

```
Input: price = [13,5,1,8,21,2], k = 3
Output: 8
Explanation: Choose the candies with the prices [13,5,21].
The tastiness of the candy basket is: min(|13 - 5|, |13 - 21|, |5 - 21|) = min(8, 8, 16) = 8.
It can be proven that 8 is the maximum tastiness that can be achieved.
```

**Example 2**

```
Input: price = [1,3,1], k = 2
Output: 2
Explanation: Choose the candies with the prices [1,3].
The tastiness of the candy basket is: min(|1 - 3|) = min(2) = 2.
It can be proven that 2 is the maximum tastiness that can be achieved.
```

**Example 3**

```
Input: price = [7,7,7,7], k = 2
Output: 0
Explanation: Choosing any two distinct candies from the candies we have will result in a tastiness of 0.
```

## Method 1

```tex
【O(nlog(n)+nlog(U)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.MaximizeMinimum;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/05/18
 */
public class MaximumTastinessOfCandyBasket {
    
    public static int maximumTastiness(int[] price, int k) {
        // Sort the prices to enable binary search and greedy checking
        Arrays.sort(price);

        // Initialize binary search boundaries:
        // left = 0 means minimum possible difference
        // right = max possible difference between prices divided by (k-1), plus 1 for upper bound
        // Explanation:
        // - The maximum minimum difference can't be larger than the total price range divided by (k-1)
        // - We add 1 to make sure right is an exclusive upper bound for binary search
        int left = 0;
        int right = (price[price.length - 1] - price[0]) / (k - 1) + 1;

        // Binary search for the maximum minimum difference (tastiness)
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;

            // Check if it's possible to select k candies such that
            // the minimum difference between any two selected candies >= mid
            if (check(price, k, mid)) {
                // If possible, we try to find a larger minimum difference
                left = mid;
            } else {
                // If not possible, we reduce the minimum difference
                right = mid;
            }
        }

        // 'left' now holds the largest minimum difference that can be achieved
        return left;
    }

    private static boolean check(int[] price, int k, int d) {
        int count = 1;          // Already selected the first candy (smallest price)
        int prev = price[0];    // The price of the last selected candy

        // Iterate through prices to greedily select candies
        for (int i = 1; i < price.length; i++) {
            // If the current candy's price is at least d away from previously selected candy
            if (price[i] - prev >= d) {
                count++;        // Select this candy
                prev = price[i]; // Update last selected candy price

                // If we have selected enough candies, return true immediately
                if (count >= k) {
                    return true;
                }
            }
        }

        // Not possible to select k candies with minimum difference d
        return false;
    }

    public static void main(String[] args) {

        int[] price1 = {13, 5, 1, 8, 21, 2};
        int k1 = 3;
        System.out.println("Example 1 maximum tastiness: " + maximumTastiness(price1, k1));  // Expected output: 8

        int[] price2 = {1, 3, 1};
        int k2 = 2;
        System.out.println("Example 2 maximum tastiness: " + maximumTastiness(price2, k2));  // Expected output: 2

        int[] price3 = {7, 7, 7, 7};
        int k3 = 2;
        System.out.println("Example 3 maximum tastiness: " + maximumTastiness(price3, k3));  // Expected output: 0
    }
}

```





