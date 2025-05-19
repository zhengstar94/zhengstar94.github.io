---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3024. Type of Triangle"
date: "2025-05-19"
tags: Easy
categories:
  - "LeetCode Array"
---



- You are given a **0-indexed** integer array `nums` of size `3` which can form the sides of a triangle.
  - A triangle is called **equilateral** if it has all sides of equal length.
  - A triangle is called **isosceles** if it has exactly two sides of equal length.
  - A triangle is called **scalene** if all its sides are of different lengths.
- Return *a string representing* *the type of triangle that can be formed* *or* `"none"` *if it **cannot** form a triangle.*

**Example 1**

```
Input: nums = [3,3,3]
Output: "equilateral"
Explanation: Since all the sides are of equal length, therefore, it will form an equilateral triangle.
```

**Example 2**

```
Input: nums = [3,4,5]
Output: "scalene"
Explanation: 
nums[0] + nums[1] = 3 + 4 = 7, which is greater than nums[2] = 5.
nums[0] + nums[2] = 3 + 5 = 8, which is greater than nums[1] = 4.
nums[1] + nums[2] = 4 + 5 = 9, which is greater than nums[0] = 3. 
Since the sum of the two sides is greater than the third side for all three cases, therefore, it can form a triangle.
As all the sides are of different lengths, it will form a scalene triangle.
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/05/19
 */
public class TypeOfTriangle {

    public static String triangleType(int[] nums) {
        // Check if the sides can form a valid triangle
        // Triangle inequality theorem: The sum of the lengths of any two sides must be greater than the length of the remaining side
        if (nums[0] + nums[1] <= nums[2] ||
                nums[0] + nums[2] <= nums[1] ||
                nums[1] + nums[2] <= nums[0]) {
            return "none";  // Cannot form a valid triangle
        }

        // Determine the type of triangle based on the equality of sides
        if (nums[0] == nums[1] && nums[1] == nums[2]) {
            return "equilateral";  // All three sides are equal -> Equilateral triangle
        } else if (nums[0] == nums[1] || nums[1] == nums[2] || nums[0] == nums[2]) {
            return "isosceles";    // Exactly two sides are equal -> Isosceles triangle
        } else {
            return "scalene";      // All three sides have different lengths -> Scalene triangle
        }
    }
    
    public static void main(String[] args) {
        // Test Case 1: Equilateral triangle (all sides equal)
        int[] nums1 = {3, 3, 3};
        System.out.println("Test Case 1: [3,3,3] -> " + triangleType(nums1));

        // Test Case 2: Scalene triangle (all sides different)
        int[] nums2 = {3, 4, 5};
        System.out.println("Test Case 2: [3,4,5] -> " + triangleType(nums2));

        // Test Case 3: Isosceles triangle (exactly two sides equal)
        int[] nums3 = {3, 3, 2};
        System.out.println("Test Case 3: [3,3,2] -> " + triangleType(nums3));

        // Test Case 4: Not a valid triangle (fails triangle inequality theorem)
        int[] nums4 = {1, 1, 3};
        System.out.println("Test Case 4: [1,1,3] -> " + triangleType(nums4));
    }
}
```





