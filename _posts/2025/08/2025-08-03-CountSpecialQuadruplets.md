---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1995. Count Special Quadruplets"
date: "2025-08-03"
tags: Easy
categories:
  - "LeetCode HashTable"
---



- Given a **0-indexed** integer array `nums`, return *the number of **distinct** quadruplets* `(a, b, c, d)` *such that:*
  - `nums[a] + nums[b] + nums[c] == nums[d]`, and
  - `a < b < c < d`

**Example 1**

```
Input: nums = [1,2,3,6]
Output: 1
Explanation: The only quadruplet that satisfies the requirement is (0, 1, 2, 3) because 1 + 2 + 3 == 6.
```

**Example 2**

```
Input: nums = [3,3,6,4,5]
Output: 0
Explanation: There are no such quadruplets in [3,3,6,4,5].
```

**Example 3**

```
Input: nums = [1,1,1,3,5]
Output: 4
Explanation: The 4 quadruplets that satisfy the requirement are:
- (0, 1, 2, 3): 1 + 1 + 1 == 3
- (0, 1, 3, 4): 1 + 1 + 3 == 5
- (0, 2, 3, 4): 1 + 1 + 3 == 5
- (1, 2, 3, 4): 1 + 1 + 3 == 5
```

## Method 1

```tex
【O(n^2) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author zhengxingxing
 * @Date 2025/08/03
 */
public class CountSpecialQuadruplets {

    /**
     * Counts the number of distinct quadruplets (a, b, c, d) such that:
     * nums[a] + nums[b] + nums[c] == nums[d] and a < b < c < d
     */
    public static int countQuadruplets(int[] nums) {
        int n = nums.length;
        int ans = 0;

        // This HashMap will store the count of each possible value of (nums[d] - nums[b+1])
        // for all valid d and fixed b. The key is the difference, the value is the count.
        Map<Integer, Integer> cnt = new HashMap<>();

        // Loop for b from n-3 down to 1.
        // Why n-3? Because we need at least two elements after b: one for c (which is b+1), one for d (which is at least b+2).
        // Why down to 1? Because a must be less than b, so b cannot be 0.
        for (int b = n - 3; b >= 1; --b) {

            // For each b, we want to consider all possible d > b+1 (so c = b+1, d > c).
            // For each such d, we calculate nums[d] - nums[b+1] and count its occurrences.
            // This is because the original equation can be rearranged as:
            // nums[a] + nums[b] = nums[d] - nums[c], where c = b+1.
            for (int d = b + 2; d < n; ++d) {
                int diff = nums[d] - nums[b + 1];
                // Increment the count for this difference in the map.
                cnt.put(diff, cnt.getOrDefault(diff, 0) + 1);
            }

            // Now, for all a < b, we check if nums[a] + nums[b] matches any previously seen difference.
            // If so, it means there exists at least one (c, d) pair such that
            // nums[a] + nums[b] + nums[c] == nums[d] with a < b < c < d.
            for (int a = 0; a < b; ++a) {
                ans += cnt.getOrDefault(nums[a] + nums[b], 0);
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        int[] nums1 = {1,2,3,6};
        System.out.println(countQuadruplets(nums1)); // Output: 1

        int[] nums2 = {3,3,6,4,5};
        System.out.println(countQuadruplets(nums2)); // Output: 0

        int[] nums3 = {1,1,1,3,5};
        System.out.println(countQuadruplets(nums3)); // Output: 4
    }
}

```





