---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "41.First Missing Positive"
date: "2024-02-16"
categories:
  - "LeetCode HashTable"
---

# LeetCode 41. First Missing Positive

- Given an unsorted integer array `nums`, return the smallest missing positive integer.
- You must implement an algorithm that runs in `O(n)` time and uses `O(1)` auxiliary space.


**Example 1**
```
Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.
```

**Example 2**

```
Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.
```

**Example 3**

```
Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.HashTable;

/**
 * @author zhengstars
 * @date 2024/02/22
 */
public class FirstMissingPositive {

    public static int firstMissingPositive(int[] nums) {
        int n = nums.length;

        // First traversal
        // Make the value at the index equals to the index plus 1
        for (int i = 0; i < n; ++i) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                // Swap nums[i] with nums[nums[i] - 1]
                int temp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
            }
        }

        // Second traversal
        // Find the first index that the value does not equal to the index plus 1
        for (int i = 0; i < n; ++i) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }

        // If all numbers are at the correct position
        // Return n + 1
        return n + 1;
    }

    public static void main(String[] args) {
        int[] nums1 = {1,2,0};
        System.out.println(firstMissingPositive(nums1));  // 3

        int[] nums2 = {3,4,-1,1};
        System.out.println(firstMissingPositive(nums2));  // 2

        int[] nums3 = {7,8,9,11,12};
        System.out.println(firstMissingPositive(nums3));  // 1
    }
}

```

