---
toc:
beginning: true
giscus_comments: true
layout: post
title: "3371. Identify the Largest Outlier in an Array"
date: "2025-07-30"
tags: Medium
categories:
    - "LeetCode HashTable"
---


- You are given an integer array `nums`. This array contains `n` elements, where **exactly** `n - 2` elements are **special** **numbers**. One of the remaining **two** elements is the *sum* of these **special numbers**, and the other is an **outlier**.
- An **outlier** is defined as a number that is *neither* one of the original special numbers *nor* the element representing the sum of those numbers.
- **Note** that special numbers, the sum element, and the outlier must have **distinct** indices, but *may* share the **same** value.
- Return the **largest** potential **outlier** in `nums`.

**Example 1**

```
Input: nums = [2,3,5,10]

Output: 10

Explanation:

The special numbers could be 2 and 3, thus making their sum 5 and the outlier 10.
```

**Example 2**

```
Input: nums = [-2,-1,-3,-6,4]

Output: 4

Explanation:

The special numbers could be -2, -1, and -3, thus making their sum -6 and the outlier 4.
```

**Example 3**

```
Input: nums = [1,1,1,1,1,5,5]

Output: 5

Explanation:

The special numbers could be 1, 1, 1, 1, and 1, thus making their sum 5 and the other 5 as the outlier.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author zhengxingxing
 * @Date 2025/07/30
 */
public class IdentifyTheLargestOutlierInAnArray {

    public static int getLargestOutlier(int[] nums) {
        // Create a HashMap to count the occurrences of each number in the array.
        Map<Integer, Integer> cnt = new HashMap<>();
        int total = 0;
        // Calculate the total sum of the array and populate the count map.
        for (int num: nums) {
            cnt.merge(num, 1, Integer::sum); // Equivalent to cnt[num]++
            total += num;
        }

        int ans = Integer.MIN_VALUE;
        // Iterate through each element, treating it as a possible outlier.
        for (int x : nums) {
            // (total - x) must be even for y to be an integer.
            // This ensures that y = (total - x) / 2 is a valid integer.
            if ((total - x) % 2 == 0) {
                int y = (total - x) / 2;
                /*
                 * Explanation of this block:
                 * - We are considering x as the possible outlier.
                 * - According to the problem, the sum of all numbers is: total = 2 * y + x,
                 *   where y is the sum of all "special numbers".
                 * - Rearranging gives y = (total - x) / 2.
                 * - We need to check if y exists in the array (cnt.containsKey(y)).
                 * - Additionally, if y == x, we must ensure that there are at least two occurrences
                 *   (so that x and y are at different indices).
                 * - If these conditions are met, x is a valid outlier candidate.
                 * - We update ans to be the maximum outlier found so far.
                 */
                if (cnt.containsKey(y) && (y != x || cnt.get(y) > 1)) {
                    ans = Math.max(ans, x);
                }
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        int[] nums1 = {2, 3, 5, 10};
        int[] nums2 = {-2, -1, -3, -6, 4};
        int[] nums3 = {1, 1, 1, 1, 1, 5, 5};

        System.out.println(getLargestOutlier(nums1)); // Output: 10
        System.out.println(getLargestOutlier(nums2)); // Output: 4
        System.out.println(getLargestOutlier(nums3)); // Output: 5
    }
}

```





