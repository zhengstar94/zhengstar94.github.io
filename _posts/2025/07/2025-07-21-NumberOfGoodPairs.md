---
toc:
beginning: true
giscus_comments: true
layout: post
title: "1512. Number of Good Pairs"
date: "2025-07-21"
tags: Easy
categories:
    - "LeetCode HashTable"
---


- Given an array of integers `nums`, return *the number of **good pairs***.
- A pair `(i, j)` is called *good* if `nums[i] == nums[j]` and `i` < `j`.

**Example 1**

```
Input: nums = [1,2,3,1,1,3]
Output: 4
Explanation: There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.
```

**Example 2**

```
Input: nums = [1,1,1,1]
Output: 6
Explanation: Each pair in the array are good.
```

**Example 3**

```
Input: nums = [1,2,3]
Output: 0
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;

/**
 * @Author zhengxingxing
 * @Date 2025/07/21
 */
public class NumberOfGoodPairs {

    public static int numIdenticalPairs(int[] nums) {
        // Create a HashMap to store the count of each number encountered so far.
        HashMap<Integer, Integer> countMap = new HashMap<>();

        // This variable will store the total number of good pairs found.
        int result = 0;

        // Iterate through each number in the input array.
        for (int num : nums) {
            // For the current number, get how many times it has appeared before.
            // Each previous occurrence can form a good pair with the current index.
            result += countMap.getOrDefault(num, 0);

            // Update the count of the current number in the map.
            // If the number is not present, start from 0 and add 1.
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }

        // After processing all numbers, return the total count of good pairs.
        return result;
    }

    public static void main(String[] args) {
        // Test case 1: There are 4 good pairs: (0,3), (0,4), (3,4), (2,5)
        int[] nums1 = {1,2,3,1,1,3};
        // Test case 2: All pairs are good, total is 6
        int[] nums2 = {1,1,1,1};
        // Test case 3: No good pairs
        int[] nums3 = {1,2,3};

        // Print the results for each test case
        System.out.println(numIdenticalPairs(nums1)); // Output: 4
        System.out.println(numIdenticalPairs(nums2)); // Output: 6
        System.out.println(numIdenticalPairs(nums3)); // Output: 0
    }
}

```





