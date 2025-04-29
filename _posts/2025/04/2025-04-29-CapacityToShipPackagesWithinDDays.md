---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1011. Capacity To Ship Packages Within D Days"
date: "2025-04-29"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.FindMinimum"
---


- A conveyor belt has packages that must be shipped from one port to another within `days` days.
- The `ith` package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.
- Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

**Example 1**

```
Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15
Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.
```

**Example 2**

```
Input: weights = [3,2,2,4,1,4], days = 3
Output: 6
Explanation: A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4
```

**Example 3**

```
Input: weights = [1,2,3,1,1], days = 4
Output: 3
Explanation:
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
```

## Method 1

```tex
【O(n * log(sum-max)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.FindMinimum;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/29
 */
public class CapacityToShipPackagesWithinDDays {

    public static int shipWithinDays(int[] weights, int days) {
        // Initialize binary search boundaries
        // left = maximum single package weight (minimum possible capacity)
        int left = Arrays.stream(weights).max().getAsInt();
        // right = sum of all weights (maximum possible capacity)
        int right = Arrays.stream(weights).sum();

        // Binary search to find minimum capacity
        while (left < right){
            // Calculate middle point avoiding overflow
            int mid = left + (right - left)/2;

            // If current capacity is feasible, try lower capacity
            if (canShipWithinDays(weights, days, mid)){
                right = mid;
            }else {
                // If current capacity is not enough, need higher capacity
                left = mid + 1;
            }
        }
        // Return the minimum feasible capacity
        return left;
    }
    
    private static boolean canShipWithinDays(int[] weights, int days, int capacity) {
        // Start with day 1
        int currentDays = 1;
        // Track current day's load
        int currentLoad = 0;

        // Process each package
        for (int weight: weights) {
            // If adding current package exceeds capacity
            if (currentLoad + weight > capacity){
                // Move to next day
                currentDays++;
                // Start new day with current package
                currentLoad = weight;

                // If exceeded allowed days, return false
                if (currentDays > days){
                    return false;
                }
            }else{
                // Add package to current day's load
                currentLoad += weight;
            }
        }

        // Return true if all packages processed within allowed days
        return true;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Multiple packages with sequential weights
        int[] weights1 = {1,2,3,4,5,6,7,8,9,10};
        int days1 = 5;
        System.out.println("Test Case 1 Result: " +
                shipWithinDays(weights1, days1));  // Expected output: 15

        // Test Case 2: Mixed weights with shorter duration
        int[] weights2 = {3,2,2,4,1,4};
        int days2 = 3;
        System.out.println("Test Case 2 Result: " +
                shipWithinDays(weights2, days2));  // Expected output: 6

        // Test Case 3: Small weights with more days
        int[] weights3 = {1,2,3,1,1};
        int days3 = 4;
        System.out.println("Test Case 3 Result: " +
                shipWithinDays(weights3, days3));  // Expected output: 3

        // Detailed explanation of Test Case 1
        System.out.println("\nShipping plan for Test Case 1:");
        System.out.println("With capacity 15:");
        System.out.println("Day 1: 1,2,3,4,5 (total=15)");
        System.out.println("Day 2: 6,7 (total=13)");
        System.out.println("Day 3: 8 (total=8)");
        System.out.println("Day 4: 9 (total=9)");
        System.out.println("Day 5: 10 (total=10)");
    }
}
```





