---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "11.Container With Most Water"
date: "2024-02-10"
categories:
  - "LeetCode Two Pointers"
---

# LeetCode 11. Container With Most Water ★★

- You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.
- Find two lines that together with the x-axis form a container, such that the container contains the most water.
- Return *the maximum amount of water a container can store*.
- **Notice** that you may not slant the container.

**Example 1**

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

**Example 2**

```
Input: height = [1,1]
Output: 1
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengstars
 * @date 2024/02/14
 */
public class ContainerWithMostWater {
    public static int maxArea(int[] height) {
        // The left pointer starts at the beginning of the array
        int left = 0;
        // The right pointer starts at the end of the array
        int right = height.length - 1;
        // Initialize the maximum area as zero
        int maxArea = 0;

        // Loop until the pointers meet
        while (left < right) {
            // For each pair of points, calculate the area and compare it with the previous maximum area
            // The area is calculated as the shorter height multiplied by the distance between the points
            maxArea = Math.max(maxArea, (right - left) * Math.min(height[left], height[right]));
            // Move the pointer that points to the shorter height
            // This is done because a higher height might be found by moving the pointer
            if (height[left] < height[right]) {
                // If the left height is shorter, move the left pointer to the right
                left++;
            } else {
                // If the right height is shorter or equal, move the right pointer to the left
                right--;
            }
        }

        // After scanning the entire array, return the maximum area found
        return maxArea;
    }

    public static void main(String[] args) {

        // Test case 1: a regular example
        // The expected output is 49, calculated as min(8, 7) * (8 - 1) = 49
        int[] arr1 = {1,8,6,2,5,4,8,3,7};
        System.out.println(maxArea(arr1));

        // Test case 2: a small example
        // The expected output is 1, calculated as min(1, 1) * (1 - 0) = 1
        int[] arr2 = {1,1};
        System.out.println(maxArea(arr2));
    }

}

```

