---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1865. Finding Pairs With a Certain Sum"
date: "2025-07-06"
tags: Medium
categories:
  - "LeetCode HashTable"
---


- You are given two integer arrays `nums1` and `nums2`. You are tasked to implement a data structure that supports queries of two types:
  - **Add** a positive integer to an element of a given index in the array `nums2`.
  - **Count** the number of pairs `(i, j)` such that `nums1[i] + nums2[j]` equals a given value (`0 <= i < nums1.length` and `0 <= j < nums2.length`).
- Implement the `FindSumPairs` class:
  - `FindSumPairs(int[] nums1, int[] nums2)` Initializes the `FindSumPairs` object with two integer arrays `nums1` and `nums2`.
  - `void add(int index, int val)` Adds `val` to `nums2[index]`, i.e., apply `nums2[index] += val`.
  - `int count(int tot)` Returns the number of pairs `(i, j)` such that `nums1[i] + nums2[j] == tot`.

**Example 1**

```
Input
["FindSumPairs", "count", "add", "count", "count", "add", "add", "count"]
[ [ [ 1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4 ] ], [7], [3, 2], [8], [4], [0, 1], [1, 1], [7 ] ]
Output
[null, 8, null, 2, 1, null, null, 11]

Explanation
FindSumPairs findSumPairs = new FindSumPairs( [ 1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4 ] );
findSumPairs.count(7);  // return 8; pairs (2,2), (3,2), (4,2), (2,4), (3,4), (4,4) make 2 + 5 and pairs (5,1), (5,5) make 3 + 4
findSumPairs.add(3, 2); // now nums2 = [1,4,5,4,5,4]
findSumPairs.count(8);  // return 2; pairs (5,2), (5,4) make 3 + 5
findSumPairs.count(4);  // return 1; pair (5,0) makes 3 + 1
findSumPairs.add(0, 1); // now nums2 = [2,4,5,4,5,4]
findSumPairs.add(1, 1); // now nums2 = [2,5,5,4,5,4]
findSumPairs.count(7);  // return 11; pairs (2,1), (2,2), (2,4), (3,1), (3,2), (3,4), (4,1), (4,2), (4,4) make 2 + 5 and pairs (5,3), (5,5) make 3 + 4
```

## Method 1

```tex
【O(m) time | O(m + q) space】
```

```java
package Leetcode.HashTable;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @Author zhengxingxing
 * @Date 2025/07/06
 */
public class FindingPairsWithACertainSum {
    // Store the first array as is, since it is not modified after initialization
    private final int[] nums1;
    // Store the second array, which can be modified by add operations
    private final int[] nums2;
    // HashMap to record the frequency of each number in nums2
    // Key: number in nums2, Value: how many times it appears in nums2
    private final Map<Integer, Integer> cnt = new HashMap<>();

    public FindingPairsWithACertainSum(int[] nums1, int[] nums2) {
        this.nums1 = nums1;
        this.nums2 = nums2;
        // Build the frequency map for nums2
        for (int x : nums2) {
            // For each number x in nums2, increment its count in the map by 1.
            // If x is not present, it will be added with value 1.
            // If x is already present, its value will be increased by 1.
            cnt.merge(x, 1, Integer::sum);
        }
    }


    public void add(int index, int val) {
        // Step 1: Decrease the count of the old value at nums2[index] in the frequency map.
        // This is because the value at this index is about to change.
        cnt.merge(nums2[index], -1, Integer::sum);

        // Step 2: Update nums2[index] by adding val to it.
        nums2[index] += val;

        // Step 3: Increase the count of the new value at nums2[index] in the frequency map.
        // This ensures the map is always up-to-date with the current nums2.
        cnt.merge(nums2[index], 1, Integer::sum);
    }

    public int count(int tot) {
        int ans = 0;
        // For each element x in nums1, we want to find how many y in nums2 satisfy x + y == tot,
        // which is equivalent to y == tot - x.
        for (int x : nums1) {
            // cnt.getOrDefault(tot - x, 0) gives the number of times (tot - x) appears in nums2.
            // Add this count to the answer.
            ans += cnt.getOrDefault(tot - x, 0);
        }
        return ans;
    }


    public static void main(String[] args) {
        List<Object> output = new ArrayList<>();
        // Initialize the object with the given nums1 and nums2 arrays.
        FindingPairsWithACertainSum obj = new FindingPairsWithACertainSum(
                new int[]{1, 1, 2, 2, 2, 3},
                new int[]{1, 4, 5, 2, 5, 4}
        );
        output.add(null); // Constructor does not return a value.

        // Perform count and add operations as per the example.
        output.add(obj.count(7)); // Returns 8
        obj.add(3, 2); output.add(null); // No return value
        output.add(obj.count(8)); // Returns 2
        output.add(obj.count(4)); // Returns 1
        obj.add(0, 1); output.add(null); // No return value
        obj.add(1, 1); output.add(null); // No return value
        output.add(obj.count(7)); // Returns 11

        // Print the output list, which matches the expected output format.
        System.out.println(output);
    }
}

```





