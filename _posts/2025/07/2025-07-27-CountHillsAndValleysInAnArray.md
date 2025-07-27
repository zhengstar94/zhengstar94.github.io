---
toc:
beginning: true
giscus_comments: true
layout: post
title: "2210. Count Hills and Valleys in an Array"
date: "2025-07-27"
tags: Easy
categories:
    - "LeetCode Array"
---


- You are given a **0-indexed** integer array `nums`. An index `i` is part of a **hill** in `nums` if the closest non-equal neighbors of `i` are smaller than `nums[i]`. Similarly, an index `i` is part of a **valley** in `nums` if the closest non-equal neighbors of `i` are larger than `nums[i]`. Adjacent indices `i` and `j` are part of the **same** hill or valley if `nums[i] == nums[j]`.
- Note that for an index to be part of a hill or valley, it must have a non-equal neighbor on **both** the left and right of the index.
- Return *the number of hills and valleys in* `nums`.

**Example 1**

```
Input: nums = [2,4,1,1,6,5]
Output: 3
Explanation:
At index 0: There is no non-equal neighbor of 2 on the left, so index 0 is neither a hill nor a valley.
At index 1: The closest non-equal neighbors of 4 are 2 and 1. Since 4 > 2 and 4 > 1, index 1 is a hill. 
At index 2: The closest non-equal neighbors of 1 are 4 and 6. Since 1 < 4 and 1 < 6, index 2 is a valley.
At index 3: The closest non-equal neighbors of 1 are 4 and 6. Since 1 < 4 and 1 < 6, index 3 is a valley, but note that it is part of the same valley as index 2.
At index 4: The closest non-equal neighbors of 6 are 1 and 5. Since 6 > 1 and 6 > 5, index 4 is a hill.
At index 5: There is no non-equal neighbor of 5 on the right, so index 5 is neither a hill nor a valley. 
There are 3 hills and valleys so we return 3.
```

**Example 2**

```
Input: nums = [6,6,5,5,4,1]
Output: 0
Explanation:
At index 0: There is no non-equal neighbor of 6 on the left, so index 0 is neither a hill nor a valley.
At index 1: There is no non-equal neighbor of 6 on the left, so index 1 is neither a hill nor a valley.
At index 2: The closest non-equal neighbors of 5 are 6 and 4. Since 5 < 6 and 5 > 4, index 2 is neither a hill nor a valley.
At index 3: The closest non-equal neighbors of 5 are 6 and 4. Since 5 < 6 and 5 > 4, index 3 is neither a hill nor a valley.
At index 4: The closest non-equal neighbors of 4 are 5 and 1. Since 4 < 5 and 4 > 1, index 4 is neither a hill nor a valley.
At index 5: There is no non-equal neighbor of 1 on the right, so index 5 is neither a hill nor a valley.
There are 0 hills and valleys so we return 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/07/27
 */
public class CountHillsAndValleysInAnArray {

    public static int countPeaksAndValleys(int[] nums) {
        int n = nums.length;
        int count = 0; // This variable will store the total number of peaks and valleys found.
        int i = 1; // Start from the second element, since the first element cannot be a peak or valley.

        // Loop through the array, but skip the first and last elements,
        // because they cannot be peaks or valleys (they don't have both neighbors).
        while (i < n - 1){
            // If the current element is equal to the previous one,
            // skip it to avoid counting the same peak/valley multiple times.
            if (nums[i] == nums[i - 1]){
                i++;
                continue;
            }

            // Find the closest non-equal neighbor to the left of nums[i].
            int left = i - 1;
            // Move left pointer to the left until a different value is found or the start of the array is reached.
            while (left >= 0 && nums[left] == nums[i]){
                left--;
            }

            // Find the closest non-equal neighbor to the right of nums[i].
            int right = i + 1;
            // Move right pointer to the right until a different value is found or the end of the array is reached.
            while (right < n && nums[right] == nums[i]){
                right++;
            }

            // Only check for peak/valley if both left and right non-equal neighbors exist.
            if (left >= 0 && right < n){
                // Check if nums[i] is a peak:
                // It is a peak if it is greater than both its closest non-equal neighbors.
                // Check if nums[i] is a valley:
                // It is a valley if it is less than both its closest non-equal neighbors.
                if((nums[i] > nums[left] && nums[i] > nums[right]) ||
                        (nums[i] < nums[left] && nums[i] < nums[right])){
                    count++; // Found a peak or valley, increment the counter.
                }
            }

            // Move i to the next different element to avoid counting the same peak/valley multiple times.
            // This is important for handling consecutive equal elements as a single peak or valley.
            i = right;
        }
        return count;
    }

    public static void main(String[] args) {
        int[] nums1 = {2, 4, 1, 1, 6, 5};
        int[] nums2 = {6, 6, 5, 5, 4, 1};
        int[] nums3 = {1, 2, 2, 2, 1, 3, 3, 1};
        System.out.println(countPeaksAndValleys(nums1)); // Output: 3
        System.out.println(countPeaksAndValleys(nums2)); // Output: 0
        System.out.println(countPeaksAndValleys(nums3)); // Output: 3
    }
}

```





