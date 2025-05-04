---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1482. Minimum Number of Days to Make m Bouquets"
date: "2025-05-04"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.FindMinimum"
---


- You are given an integer array `bloomDay`, an integer `m` and an integer `k`.
- You want to make `m` bouquets. To make a bouquet, you need to use `k` **adjacent flowers** from the garden.
- The garden consists of `n` flowers, the `ith` flower will bloom in the `bloomDay[i]` and then can be used in **exactly one** bouquet.
- Return *the minimum number of days you need to wait to be able to make* `m` *bouquets from the garden*. If it is impossible to make m bouquets return `-1`.

**Example 1**

```
Input: bloomDay = [1,10,3,10,2], m = 3, k = 1
Output: 3
Explanation: Let us see what happened in the first three days. x means flower bloomed and _ means flower did not bloom in the garden.
We need 3 bouquets each should contain 1 flower.
After day 1: [x, _, _, _, _]   // we can only make one bouquet.
After day 2: [x, _, _, _, x]   // we can only make two bouquets.
After day 3: [x, _, x, _, x]   // we can make 3 bouquets. The answer is 3.
```

**Example 2**

```
Input: bloomDay = [1,10,3,10,2], m = 3, k = 2
Output: -1
Explanation: We need 3 bouquets each has 2 flowers, that means we need 6 flowers. We only have 5 flowers so it is impossible to get the needed bouquets and we return -1.
```

**Example 3**

```
Input: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
Output: 12
Explanation: We need 2 bouquets each should have 3 flowers.
Here is the garden after the 7 and 12 days:
After day 7: [x, x, x, x, _, x, x]
We can make one bouquet of the first three flowers that bloomed. We cannot make another bouquet from the last three flowers that bloomed because they are not adjacent.
After day 12: [x, x, x, x, x, x, x]
It is obvious that we can make two bouquets in different ways.
```

## Method 1

```tex
【O(n * log(max-min) time | O(1) space】
```

```java
package Leetcode.BinarySearch.FindMinimum;

/**
 * @author zhengxingxing
 * @date 2025/05/04
 */
public class MinimumNumberOfDaysToMakeMBouquets {

    public static int minDays(int[] bloomDay, int m, int k) {
        int n = bloomDay.length;

        // Check if there are enough flowers to make m bouquets
        // Each bouquet needs k flowers, so we need m*k flowers in total
        // Using long to prevent integer overflow for large m and k values
        if ((long) m * k > n){
            return -1;
        }

        // Initialize binary search boundaries
        // left: earliest possible day (minimum bloom day)
        // right: latest possible day (maximum bloom day)
        int left = Integer.MAX_VALUE;
        int right = Integer.MIN_VALUE;

        // Find the minimum and maximum bloom days from the array
        // These values will serve as our binary search range
        for (int day: bloomDay) {
            left = Math.min(left, day);
            right = Math.max(right, day);
        }

        // Perform binary search to find the minimum number of days
        // The search space is between the earliest and latest bloom days
        while (left < right){
            // Calculate the middle day to test
            // Using (right-left)/2 to avoid integer overflow
            int mid = left + (right - left) / 2;

            // Check if we can make m bouquets by waiting 'mid' days
            // If possible, try to minimize by searching in the lower half
            if (canMakeBouquets(bloomDay, m, k, mid)) {
                right = mid;
            } else {
                // Otherwise, we need more days, search in the upper half
                left = mid + 1;
            }
        }

        // At this point, 'left' contains the minimum number of days needed
        return left;
    }

    private static boolean canMakeBouquets(int[] bloomDay, int m, int k, int days) {
        // Counter for the number of bouquets we can make
        int bouquets = 0;
        // Counter for the current sequence of adjacent bloomed flowers
        int flowers = 0;

        // Iterate through each flower in the garden
        for (int i = 0; i < bloomDay.length; i++) {
            // If this flower has bloomed by 'days' day
            if (bloomDay[i] <= days) {
                // Increment the count of consecutive bloomed flowers
                flowers++;

                // If we have k adjacent bloomed flowers, we can make a bouquet
                if (flowers == k) {
                    // Make a bouquet and reset the flower counter to start a new bouquet
                    bouquets++;
                    flowers = 0;
                }
            } else {
                // If this flower hasn't bloomed, we break the consecutive sequence
                // Reset the flower counter since we need k adjacent bloomed flowers
                flowers = 0;
            }

            // Early termination: if we've made enough bouquets, return true
            // This optimization saves computation when we've already found the answer
            if (bouquets >= m) {
                return true;
            }
        }

        // Check if we've made at least m bouquets
        return bouquets >= m;
    }

    public static void main(String[] args) {
        // Example 1: bloomDay = [1,10,3,10,2], m = 3, k = 1, Expected output: 3
        int[] bloomDay1 = {1, 10, 3, 10, 2};
        int m1 = 3;
        int k1 = 1;
        System.out.println("Example 1:");
        System.out.println("Input: bloomDay = [1,10,3,10,2], m = 3, k = 1");
        System.out.println("Output: " + minDays(bloomDay1, m1, k1));
        System.out.println("Expected: 3");
        System.out.println();

        // Example 2: bloomDay = [1,10,3,10,2], m = 3, k = 2, Expected output: -1
        int[] bloomDay2 = {1, 10, 3, 10, 2};
        int m2 = 3;
        int k2 = 2;
        System.out.println("Example 2:");
        System.out.println("Input: bloomDay = [1,10,3,10,2], m = 3, k = 2");
        System.out.println("Output: " + minDays(bloomDay2, m2, k2));
        System.out.println("Expected: -1");
        System.out.println();

        // Example 3: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3, Expected output: 12
        int[] bloomDay3 = {7, 7, 7, 7, 12, 7, 7};
        int m3 = 2;
        int k3 = 3;
        System.out.println("Example 3:");
        System.out.println("Input: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3");
        System.out.println("Output: " + minDays(bloomDay3, m3, k3));
        System.out.println("Expected: 12");
        System.out.println();

        // Example 4: bloomDay = [1000000000,1000000000], m = 1, k = 1, Expected output: 1000000000
        int[] bloomDay4 = {1000000000, 1000000000};
        int m4 = 1;
        int k4 = 1;
        System.out.println("Example 4:");
        System.out.println("Input: bloomDay = [1000000000,1000000000], m = 1, k = 1");
        System.out.println("Output: " + minDays(bloomDay4, m4, k4));
        System.out.println("Expected: 1000000000");
        System.out.println();

        // Example 5: bloomDay = [1,10,2,9,3,8,4,7,5,6], m = 4, k = 2, Expected output: 9
        int[] bloomDay5 = {1, 10, 2, 9, 3, 8, 4, 7, 5, 6};
        int m5 = 4;
        int k5 = 2;
        System.out.println("Example 5:");
        System.out.println("Input: bloomDay = [1,10,2,9,3,8,4,7,5,6], m = 4, k = 2");
        System.out.println("Output: " + minDays(bloomDay5, m5, k5));
        System.out.println("Expected: 9");
    }
}

```





