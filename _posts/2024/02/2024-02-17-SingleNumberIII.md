---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "260.Single Number III"
date: "2024-02-17"
categories:
  - "LeetCode HashTable"
---

# LeetCode 260. Single Number III

- Given an integer array `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in **any order**.
- You must write an algorithm that runs in linear runtime complexity and uses only constant extra space.


**Example 1**

```
Input: nums = [1,2,1,3,2,5]
Output: [3,5]
Explanation:  [5, 3] is also a valid answer.
```

**Example 2**

```
Input: nums = [-1,0]
Output: [-1,0]
```

**Example 3**

```
Input: nums = [0,1]
Output: [1,0]
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.HashTable;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/02/23
 */
public class SingleNumberIII {
    public static int[] singleNumber(int[] nums) {
        /* Step 1:
         *  By using the XOR operator, we can get the XOR result of the two unique numbers we want to find.
         *  Note that the XOR operator returns 0 for two identical numbers and returns the number itself for a number and 0.
         */
        int diff = 0;
        for (int num : nums) {
            diff ^= num;
        }
        /* Then we keep the rightmost 1-bit of the diff, which is the first different bit from right to left of the two unique numbers.
         */
        diff &= -diff;

        // Step 2:
        /* We use an array of two elements to store the two unique numbers we're going to find.
         *  We divide all the numbers into two groups, in each of which one of the two unique numbers belongs to, according to whether
         *  they have a 0 or 1 at the diff bit.
         *  We then XOR all the numbers in each group to find the unique number in each group.
         */
        int[] rets = {0, 0};
        for (int num : nums) {
            /* We check whether the number in the diff bit is 0 or not.
             *  If it is 0, the number will be XORed with rets[0].
             *  Otherwise, the number will be XORed with rets[1].
             *  Therefore, two groups of numbers are separated by whether the number in the diff bit is 0 or not.
             */
            if ((num & diff) == 0) {
                rets[0] ^= num;
            } else {
                rets[1] ^= num;
            }
        }
        // Finally, we return the array of two unique numbers.
        return rets;
    }

    public static void main(String[] args) {

        int[] nums1 = {1, 2, 1, 3, 2, 5};
        System.out.println(Arrays.toString(singleNumber(nums1))); // [3, 5]

        int[] nums2 = {-1, 0};
        System.out.println(Arrays.toString(singleNumber(nums2))); //  [-1, 0]

        int[] nums3 = {0, 1};
        System.out.println(Arrays.toString(singleNumber(nums3))); //  [0, 1]
    }
}

```

