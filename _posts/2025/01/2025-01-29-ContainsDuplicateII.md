---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "219. Contains Duplicate II"
date: "2025-01-29"
categories:
  - "LeetCode SlideWindow"
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
package Leetcode.SlideWindow;

import java.util.HashSet;

/**
 * @author zhengxingxing
 * @date 2025/01/29
 */
public class ContainsDuplicateII {

  public static boolean containsNearbyDuplicate(int[] nums, int k) {
    // Using HashSet as a sliding window to store elements
    HashSet<Integer> set = new HashSet<>();

    // Iterate through the array
    for (int i = 0; i < nums.length; i++) {
      // If window size exceeds k, remove the leftmost element
      // This maintains the window size of k elements
      if (i > k) {
        set.remove(nums[i - k - 1]);
      }

      // Try to add current element to set
      // If addition fails, it means element already exists in window
      // Therefore, we found duplicates within distance k
      if (!set.add(nums[i])) {
        return true;
      }
    }
    // No duplicates found within distance k
    return false;
  }

  public static void main(String[] args) {
    // Test Case 1: Should return true
    // Contains duplicate 1's with distance 3
    int[] nums1 = {1, 2, 3, 1};
    int k1 = 3;
    System.out.println("Test Case 1 Result: " + containsNearbyDuplicate(nums1, k1));

    // Test Case 2: Should return true
    // Contains duplicate 1's with distance 1
    int[] nums2 = {1, 0, 1, 1};
    int k2 = 1;
    System.out.println("Test Case 2 Result: " + containsNearbyDuplicate(nums2, k2));

    // Test Case 3: Should return false
    // No duplicates within distance 2
    int[] nums3 = {1, 2, 3, 1, 2, 3};
    int k3 = 2;
    System.out.println("Test Case 3 Result: " + containsNearbyDuplicate(nums3, k3));
  }
}


```





