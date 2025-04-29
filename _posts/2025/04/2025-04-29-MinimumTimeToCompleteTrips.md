---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2187. Minimum Time to Complete Trips"
date: "2025-04-29"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.FindMinimum"
---


- You are given an array `time` where `time[i]` denotes the time taken by the `ith` bus to complete **one trip**.
- Each bus can make multiple trips **successively**; that is, the next trip can start **immediately after** completing the current trip. Also, each bus operates **independently**; that is, the trips of one bus do not influence the trips of any other bus.
- You are also given an integer `totalTrips`, which denotes the number of trips all buses should make **in total**. Return *the **minimum time** required for all buses to complete **at least*** `totalTrips` *trips*.

**Example 1**

```
Input: time = [1,2,3], totalTrips = 5
Output: 3
Explanation:
- At time t = 1, the number of trips completed by each bus are [1,0,0]. 
  The total number of trips completed is 1 + 0 + 0 = 1.
- At time t = 2, the number of trips completed by each bus are [2,1,0]. 
  The total number of trips completed is 2 + 1 + 0 = 3.
- At time t = 3, the number of trips completed by each bus are [3,1,1]. 
  The total number of trips completed is 3 + 1 + 1 = 5.
So the minimum time needed for all buses to complete at least 5 trips is 3.
```

**Example 2**

```
Input: time = [2], totalTrips = 1
Output: 2
Explanation:
There is only one bus, and it will complete its first trip at t = 2.
So the minimum time needed to complete 1 trip is 2.
```

## Method 1

```tex
【O(n * log(min(time) * totalTrips)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.FindMinimum;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/29
 */
public class MinimumTimeToCompleteTrips {
    public static long minimumTime(int[] time, int totalTrips) {
        // Initialize left pointer to minimum possible time
        long left = 1;
        // Initialize right pointer to maximum possible time
        // right = minimum time among all buses * total trips required
        long right = (long) Arrays.stream(time).min().getAsInt() * totalTrips;

        // Binary search to find the minimum time
        while (left < right){
            // Calculate mid point to avoid overflow
            long mid = left + (right - left)/2;
            // If we can complete required trips in mid time, try to minimize the time
            if (canComplete(time, mid, totalTrips)){
                right = mid;
            } else {
                // If we cannot complete in mid time, we need more time
                left = mid + 1;
            }
        }

        // Return the minimum time found
        return left;
    }
    
    private static boolean canComplete(int[] time, long givenTime, int totalTrips) {
        // Count total trips possible in given time
        long trip = 0;
        // Calculate trips for each bus
        for (int t: time) {
            // Add number of trips this bus can complete
            trip += givenTime / t;
            // Early return if we already reached required trips
            if (trip >= totalTrips){
                return true;
            }
        }
        // Return false if total trips possible is less than required
        return false;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Multiple buses with different times
        int[] time1 = {1, 2, 3};
        int totalTrips1 = 5;
        System.out.println("Test Case 1 Result: " + minimumTime(time1, totalTrips1)); // Expected output: 3

        // Test Case 2: Single bus case
        int[] time2 = {2};
        int totalTrips2 = 1;
        System.out.println("Test Case 2 Result: " + minimumTime(time2, totalTrips2)); // Expected output: 2
    }
}

```





