---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "287. Find the Duplicate Number"
date: "2024-01-21"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 287. Find the Duplicate Number [Hard]

- Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.
- There is only **one repeated number** in `nums`, return *this repeated number*.
- You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**Example 1**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

## Method 1

```tex
【O(nlog(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/02/17
 */
public class FindTheDuplicateNumber {
    public static int findDuplicate(int[] nums) {

        // Initialize two pointers, 'left' and 'right'. 'left' starts from 1 and 'right' starts from the last element of the array.
        int left = 1;
        int right = nums.length - 1;

        // Enter a loop to narrow down the difference between 'left' and 'right' into one, which will be the duplicate number
        while (left < right) {

            // Calculate the middle value of 'left' and 'right'
            int mid = (left + right) / 2;

            // Count the elements of the array which are less than or equal to the mid value
            int count = 0;
            for (int num : nums) {
                if (num <= mid) {
                    count++;
                }
            }

            // If the count is greater than the mid value, then the duplicate value is in the left half. So, adjust 'right' to 'mid'
            if (count > mid) {
                right = mid;
            }
            // Otherwise, the duplicate value is in the right half. So, adjust 'left' to 'mid' + 1
            else {
                left = mid + 1;
            }
        }

        // Once the loop ends, 'left' will be the duplicate number. Return 'left'
        return left;
    }
    
    public static void main(String[] args) {

        // Call and print the result of the findDuplicate method on the provided numerical arrays.
        System.out.println(findDuplicate(new int[]{1, 3, 4, 2, 2}));   // The output should be: 2
        System.out.println(findDuplicate(new int[]{3, 1, 3, 4, 2}));   // The output should be: 3
    }
}
```

## Method 2

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/02/17
 */
public class FindTheDuplicateNumber {
    public static int findDuplicate(int[] nums) {

        // Initialize 'slow' and 'fast' pointers.
        // 'slow' pointer starts from the first element of the array.
        // 'fast' pointer starts from the element indexed by the first element of the array.
        int slow = nums[0];
        int fast = nums[nums[0]];

        // Enter a loop to find the point where 'slow' and 'fast' meet. 
        // 'slow' moves one step at a time, while 'fast' moves two steps at a time.
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }

        // Once they meet, initialize 'fast' to 0 and keep 'slow' at the meeting point. 
        // Start another loop to find the entry point of the cycle, which is the duplicate number.
        fast = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        // Return the duplicate number.
        return slow;
    }
    
    public static void main(String[] args) {

        // Call and print the result of the findDuplicate method on the provided numerical arrays.
        System.out.println(findDuplicate(new int[]{1, 3, 4, 2, 2}));   // The output should be: 2
        System.out.println(findDuplicate(new int[]{3, 1, 3, 4, 2}));   // The output should be: 3
    }
}

```

