---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1248. Count Number of Nice Subarrays"
date: "2025-02-13"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountExact"
---


- Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.
- Return *the number of **nice** sub-arrays*.


**Example 1**

```
Input: nums = [1,1,2,1,1], k = 3
Output: 2
Explanation: The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].
```

**Example 2**

```
Input: nums = [2,4,6], k = 1
Output: 0
Explanation: There are no odd numbers in the array.
```

**Example 3**

```
Input: nums = [2,2,2,1,2,2,1,2,2,2], k = 2
Output: 16
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountExact;

/**
 * @author zhengxingxing
 * @date 2025/02/13
 */
public class CountNumberOfNiceSubarrays {
    public static int numberOfSubarrays(int[] nums, int k) {
        int res = 0;
        int count1 = 0;
        int count2 = 0;

        for (int l1 = 0, l2 = 0, r = 0; r < nums.length; r++) {
            if (nums[r] % 2 == 1){
                count1++;
                count2++;
            }

            while (l1 <= r && count1 >= k){
                if(nums[l1] % 2 == 1){
                    count1--;
                }
                l1++;
            }

            while (l2 <= r && count2 >= k + 1){
                if(nums[l2] % 2 == 1){
                    count2--;
                }
                l2++;
            }

            res += l1 - l2;
        }

        return res;
    }

    public static void main(String[] args) {
        // 测试用例1：常规情况
        int[] nums1 = {1, 1, 2, 1, 1};
        int k1 = 3;
        System.out.println("测试用例1结果: " + numberOfSubarrays(nums1, k1)); // 输出: 2

        // 测试用例2：没有奇数的情况
        int[] nums2 = {2, 4, 6};
        int k2 = 1;
        System.out.println("测试用例2结果: " + numberOfSubarrays(nums2, k2)); // 输出: 0

        // 测试用例3：较长数组
        int[] nums3 = {2, 2, 2, 1, 2, 2, 1, 2, 2, 2};
        int k3 = 2;
        System.out.println("测试用例3结果: " + numberOfSubarrays(nums3, k3)); // 输出: 16
    }
}

```





