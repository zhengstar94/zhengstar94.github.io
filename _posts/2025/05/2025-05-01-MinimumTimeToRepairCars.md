---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2594. Minimum Time to Repair Cars"
date: "2025-05-01"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.FindMinimum"
---


- You are given an integer array `ranks` representing the **ranks** of some mechanics. ranksi is the rank of the ith mechanic. A mechanic with a rank `r` can repair n cars in `r * n2` minutes.
- You are also given an integer `cars` representing the total number of cars waiting in the garage to be repaired.
- Return *the **minimum** time taken to repair all the cars.***
- **Note:** All the mechanics can repair the cars simultaneously.

**Example 1**

```
Input: ranks = [4,2,3,1], cars = 10
Output: 16
Explanation: 
- The first mechanic will repair two cars. The time required is 4 * 2 * 2 = 16 minutes.
- The second mechanic will repair two cars. The time required is 2 * 2 * 2 = 8 minutes.
- The third mechanic will repair two cars. The time required is 3 * 2 * 2 = 12 minutes.
- The fourth mechanic will repair four cars. The time required is 1 * 4 * 4 = 16 minutes.
It can be proved that the cars cannot be repaired in less than 16 minutes.
```

**Example 2**

```
Input: ranks = [5,1,8], cars = 6
Output: 16
Explanation: 
- The first mechanic will repair one car. The time required is 5 * 1 * 1 = 5 minutes.
- The second mechanic will repair four cars. The time required is 1 * 4 * 4 = 16 minutes.
- The third mechanic will repair one car. The time required is 8 * 1 * 1 = 8 minutes.
It can be proved that the cars cannot be repaired in less than 16 minutes.
```

## Method 1

```tex
【O(n * log(R * C²)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.FindMinimum;

/**
 * @author zhengxingxing
 * @date 2025/05/01
 */
public class MinimumTimeToRepairCars {

    public static long repairCars(int[] ranks, int cars) {
        // Initialize the lower bound of our search range to 1
        // The minimum possible time cannot be less than 1
        long left = 1;

        // Initialize the upper bound of our search range
        // We calculate an upper bound that is guaranteed to be larger than the answer
        // Formula: rank * cars^2 represents the time needed if one mechanic repairs all cars
        // Using ranks[0] gives us a sufficient upper bound, even if it's not the maximum rank
        // This is a key optimization: we only need a value that's larger than the actual answer
        long right = (long) ranks[0] * cars * cars;

        // Perform binary search to find the minimum possible time
        // The search continues until left and right pointers converge
        while (left < right) {
            // Calculate the middle point, avoiding potential overflow with this formula
            long mid = left + (right - left) / 2;

            // Check if all cars can be repaired within 'mid' time
            if (canRepairAll(ranks, cars, mid)) {
                // If possible to repair within 'mid' time, try a smaller time
                // We don't exclude 'mid' because it might be the answer
                right = mid;
            } else {
                // If not possible to repair within 'mid' time, we need more time
                // Exclude 'mid' as it's definitely not enough time
                left = mid + 1;
            }
        }

        // When left == right, we've found our answer
        // This is the minimum time needed to repair all cars
        return left;
    }

    private static boolean canRepairAll(int[] ranks, int cars, long time) {
        // Track the total number of cars that can be repaired within the time limit
        long totalCars = 0;

        // Iterate through each mechanic's rank to calculate their contribution
        for (int rank : ranks) {
            // For each mechanic with efficiency rank 'rank':
            // The time to repair n cars is: time = rank * n^2
            // Solving for n: n = sqrt(time/rank)
            // This gives us the maximum number of cars this mechanic can repair in the given time

            // Calculate how many cars this mechanic can repair
            // The cast to long ensures we don't lose precision in the square root calculation
            long carsPerMechanic = (long) Math.sqrt(time / rank);

            // Add this mechanic's contribution to the total
            totalCars += carsPerMechanic;

            // Early termination optimization:
            // If we've already found that enough cars can be repaired, return immediately
            if (totalCars >= cars) {
                return true;
            }
        }

        // Final check: can all mechanics combined repair enough cars?
        // This will only be reached if the early termination condition above wasn't triggered
        return totalCars >= cars;
    }

    public static void main(String[] args) {
        // Test case 1: 4 mechanics with ranks [4,2,3,1], need to repair 10 cars
        int[] ranks1 = {4, 2, 3, 1};
        int cars1 = 10;
        long result1 = repairCars(ranks1, cars1);
        System.out.println("Test case 1 result: " + result1);
        System.out.println("Expected result: 16");

        // Test case 2: 3 mechanics with ranks [5,1,8], need to repair 6 cars
        int[] ranks2 = {5, 1, 8};
        int cars2 = 6;
        long result2 = repairCars(ranks2, cars2);
        System.out.println("Test case 2 result: " + result2);
        System.out.println("Expected result: 16");

        // Explanation for test case 2 - showing the optimal distribution of cars
        System.out.println("\nTest case 2 explanation:");
        System.out.println("- The first mechanic (rank 5) repairs 1 car, taking 5 * 1 * 1 = 5 minutes.");
        System.out.println("- The second mechanic (rank 1) repairs 4 cars, taking 1 * 4 * 4 = 16 minutes.");
        System.out.println("- The third mechanic (rank 8) repairs 1 car, taking 8 * 1 * 1 = 8 minutes.");
        System.out.println("16 minutes is the minimum time needed to repair all cars.");
    }
}
```





