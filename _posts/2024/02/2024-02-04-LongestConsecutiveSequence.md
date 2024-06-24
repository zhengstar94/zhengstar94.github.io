---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "128.Longest Consecutive Sequence"
date: "2024-02-04"
categories:
  - "LeetCode Array"
---

# LeetCode 128. Longest Consecutive Sequence [Easy]

- Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*
- You must write an algorithm that runs in `O(n)` time.

**Example 1**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Array;

import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2024/03/01
 */
public class LongestConsecutiveSequence {
    public static int longestConsecutive(int[] nums) {
        // Initialize a HashSet to keep track of all individual numbers in the array
        Set<Integer> numSet = new HashSet<>();

        // Iterate over the array, adding all numbers to the set
        for (int num : nums) {
            numSet.add(num);
        }

        // Initialize a variable to keep track of the longest streak
        int longestStreak = 0;

        // Iterate over the set
        for (int num : numSet) {
            // If the set does not contain the current number minus one, it means the current number could be
            // the start of a new sequence, then we check for the sequence
            if (!numSet.contains(num - 1)) {
                // Current number of sequence
                int currentNum = num;
                // Initialize current streak
                int currentStreak = 1;

                // Find the length of current sequence by checking the existence of the next number
                while (numSet.contains(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                // Update the longest streak with maximum value of current streak or longest streak
                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        // Return the longest consecutive sequence
        return longestStreak;
    }

    public static void main(String[] args) {
        int[] nums1 = {100, 4, 200, 1, 3, 2,0};  // Test case
        System.out.println(longestConsecutive(nums1));  // Expected output: 4
    }
}
```

