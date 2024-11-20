---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "219. Contains Duplicate II"
date: "2024-11-20"
categories:
  - "LeetCode HashTable"
---


- Given an integer array `nums` and an integer `k`, return `true` *if there are two **distinct indices*** `i` *and* `j` *in the array such that* `nums[i] == nums[j]` *and* `abs(i - j) <= k`.

**Example 1**

```
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2**

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

**Example 3**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;

/**
 * @author zhengxingxing
 * @date 2024/11/20
 */
public class ContainsDuplicateII {
    public static boolean containsNearbyDuplicate(int[] nums, int k) {
        // Create a HashMap to store the value and its last occurrence index.
        HashMap<Integer, Integer> map = new HashMap<>();

        // Iterate through the array.
        for (int i = 0; i < nums.length; i++) {
            // Check if the current number already exists in the map.
            if (map.containsKey(nums[i])) {
                // Retrieve the index of the previous occurrence of nums[i].
                int prevIndex = map.get(nums[i]);

                // If the difference between the current and previous indices is <= k, return true.
                if (i - prevIndex <= k) {
                    return true;
                }
            }
            // Update the map with the current index of nums[i].
            map.put(nums[i], i);
        }

        // If no such pair is found, return false.
        return false;
    }

    public static void main(String[] args) {
        // Test case 1: Duplicate "1" within distance 3 -> Expected: true
        System.out.println(containsNearbyDuplicate(new int[]{ 1, 2, 3, 1 }, 3 )); // true

        // Test case 2: Duplicate "1" within distance 1 -> Expected: true
        System.out.println(containsNearbyDuplicate(new int[]{ 1, 0, 1, 1 }, 1 )); // true

        // Test case 3: No duplicates within the required distance -> Expected: false
        System.out.println(containsNearbyDuplicate(new int[]{ 1, 2, 3, 1, 2, 3 }, 2 )); // false
    }
}

```





