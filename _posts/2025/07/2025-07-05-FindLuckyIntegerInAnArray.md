---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1394. Find Lucky Integer in an Array"
date: "2025-07-05"
tags: Easy
categories:
  - "LeetCode HashTable"
---


- Given an array of integers `arr`, a **lucky integer** is an integer that has a frequency in the array equal to its value.
- Return *the largest **lucky integer** in the array*. If there is no **lucky integer** return `-1`.

**Example 1**

```
Input: arr = [2,2,3,4]
Output: 2
Explanation: The only lucky number in the array is 2 because frequency[2] == 2.
```

**Example 2**

```
Input: arr = [1,2,2,3,3,3]
Output: 3
Explanation: 1, 2 and 3 are all lucky numbers, return the largest of them.
```

**Example 3**

```
Input: arr = [2,2,2,3,3]
Output: -1
Explanation: There are no lucky numbers in the array.
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
 * @Date 2025/07/05
 */
public class FindLuckyIntegerInAnArray {
    public static int findLucky(int[] arr) {
        // Create a HashMap to store the frequency of each integer in the array
        Map<Integer, Integer> freq = new HashMap<>();
        // Iterate through the array and count the occurrences of each number
        for (int num : arr) {
            // If num is already in the map, increment its count by 1; otherwise, set it to 1
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }
        // Initialize the result variable to -1 (default if no lucky integer is found)
        int res = -1;
        // Iterate through all entries in the frequency map
        for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
            int num = entry.getKey();    // The integer value
            int count = entry.getValue(); // The frequency of this integer
            // Check if the integer is a lucky integer (its value equals its frequency)
            if (num == count) {
                // Update the result if this lucky integer is larger than the current result
                res = Math.max(res, num);
            }
        }
        // Return the largest lucky integer found, or -1 if none exists
        return res;
    }

    public static void main(String[] args) {
        int[] arr1 = {2, 2, 3, 4};
        int[] arr2 = {1, 2, 2, 3, 3, 3};
        int[] arr3 = {2, 2, 2, 3, 3};
        int[] arr4 = {5, 5, 5, 5, 5};
        System.out.println(findLucky(arr1)); // Output: 2
        System.out.println(findLucky(arr2)); // Output: 3
        System.out.println(findLucky(arr3)); // Output: -1
        System.out.println(findLucky(arr4)); // Output: 5
    }
}

```





