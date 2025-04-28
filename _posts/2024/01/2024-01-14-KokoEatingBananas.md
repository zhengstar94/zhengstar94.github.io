---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "875. Koko Eating Bananas"
date: "2024-01-14"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 875. Koko Eating Bananas [Medium]

- Koko loves to eat bananas. There are `N` piles of bananas, the `i`-th pile has `piles[i]` bananas. The guards have gone and will come back in `H` hours.
- Koko can decide her bananas-per-hour eating speed of `K`. Each hour, she chooses some pile of bananas, and eats K bananas from that pile. If the pile has less than `K` bananas, she eats all of them instead, and won’t eat any more bananas during this hour.
- Koko likes to eat slowly, but still wants to finish eating all the bananas before the guards come back.
- Return the minimum integer `K` such that she can eat all the bananas within `H` hours.

**Example 1**

```
Input: piles = [3,6,7,11], H = 8
Output: 4
```

**Example 2**

```
Input: piles = [30,11,23,4,20], H = 5
Output: 30
```

**Example 3**

```
Input: piles = [30,11,23,4,20], H = 6
Output: 23
```



## Method 1

```tex
【O(nlog(m))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/01/28
 */
public class KokoEatingBananas {
    public static int minEatingSpeed(int[] piles, int H) {
        // The left boundary is 1 because at least 1 banana is eaten
        int left = 1;
        // Find the maximum value in the piles array, which is the right boundary
        int right = Arrays.stream(piles).max().getAsInt();

        while (left < right) {
            // Calculate the middle value
            int mid = left + (right - left) / 2;
            int total = 0;
            // Calculate how many hours it takes to finish eating each pile if each hour eats 'mid' banana(s)
            for (int pile : piles) {
                total += (pile + mid - 1) / mid;
            }
            // If the required time is greater than 'H', the speed of eating bananas needs to be increased, 
            // that is, `mid` needs to be increased, which means the left boundary needs to be increased
            if (total > H) {
                left = mid + 1;
            } else {
                // If the required time is less than or equal to 'H', the speed can be reduced, but the current 
                // valid 'mid' needs to be recorded, which is the right boundary
                right = mid;
            }
        }
        // Return the last calculated speed, which is the minimum speed that satisfies the condition
        return left;
    }

    // Testing the function with example test cases
    public static void main(String[] args) {
        System.out.println(minEatingSpeed(new int[]{3, 6, 7, 11}, 8)); // Expected output: 4
        System.out.println(minEatingSpeed(new int[]{30, 11, 23, 4, 20}, 6)); // Expected output: 23
    }
}

```

