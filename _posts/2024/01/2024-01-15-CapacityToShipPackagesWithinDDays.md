---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1011. Capacity To Ship Packages Within D Days"
date: "2024-01-15"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 1011. Capacity To Ship Packages Within D Days [Medium]

- A conveyor belt has packages that must be shipped from one port to another within `D` days.
- The `i`-th package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.
- Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `D` days.

**Example 1**

```
Input: weights = [1,2,3,4,5,6,7,8,9,10], D = 5
Output: 15
Explanation: 
A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed. 
```

**Example 2**

```
Input: weights = [3,2,2,4,1,4], D = 3
Output: 6
Explanation: 
A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4
```

**Example 3**

```
Input: weights = [1,2,3,1,1], D = 4
Output: 3
Explanation: 
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
```



## Method 1

```tex
【O(nlog(sum(weights)))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/01/28
 */
public class Capacity {
    public static int shipWithinDays(int[] weights, int D) {
        // Determine the initial lower and upper bounds of the binary search.
        // The lower bound (left) is the maximum weight in the array.
        // The upper bound (right) is the sum of all weights.
        int left = Arrays.stream(weights).max().getAsInt();
        int right = Arrays.stream(weights).sum();

        // Perform the binary search.
        while (left < right) {
            // Calculate the middle point.
            int mid = left + (right - left) / 2;

            // Initialize the number of days needed to 1 and the current weight to 0.
            int need = 1;
            int cur = 0;

            // Loop through the weights array.
            for (int weight : weights) {
                // If the current sum of weights is greater than mid, we need to increase the day count and reset the current weight.
                if (cur + weight > mid) {
                    need++;
                    cur = 0;
                }
                // Add the weight to the current weight.
                cur += weight;
            }

            // If the number of days needed exceeds D, we need to increase the lower bound of the binary search.
            // Otherwise, we can decrease the upper bound.
            if (need > D) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // Return the optimal ship weight capacity.
        return left;
    }

    public static void main(String[] args) {
        // Test Case 1
        System.out.println(shipWithinDays(new int[]{1,2,3,4,5,6,7,8,9,10}, 5)); // Output: 15

        // Test Case 2
        System.out.println(shipWithinDays(new int[]{3,2,2,4,1,4}, 3)); // Output: 6

        // Test Case 3
        System.out.println(shipWithinDays(new int[]{1,2,3,1,1}, 4)); // Output: 3
    }
}

```

