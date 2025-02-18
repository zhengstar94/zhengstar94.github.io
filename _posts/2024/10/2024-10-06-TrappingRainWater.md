---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "42.Trapping Rain Water"
date: "2024-10-06"
categories:
  - "LeetCode TwoPointers"
---


- Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1**

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

**Example 2**

```
Input: height = [4,2,0,3,2,5]
Output: 9
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengstars
 * @date 2024/10/06
 */
public class TrappingRainWater {
    public static int trap(int[] height) {
        // Initialize two pointers: left starts from the beginning, right from the end
        int left = 0;
        int right = height.length - 1;

        // Keep track of the maximum height encountered from left and right
        int leftMax = 0;
        int rightMax = 0;

        // Variable to store the total amount of water trapped
        int result = 0;

        // Continue until the two pointers meet
        while (left < right) {
            // Compare the heights at the left and right pointers
            // This comparison is crucial as it determines which side to process
            if (height[left] < height[right]) {
                // Process the left side when its height is smaller

                // If current height is greater or equal to leftMax,
                // update leftMax as no water can be trapped here
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    // Water can be trapped at this position
                    // The amount is the difference between leftMax and current height
                    result += leftMax - height[left];
                }
                // Move the left pointer to the right
                left++;
            } else {
                // Process the right side when its height is smaller or equal

                // If current height is greater or equal to rightMax,
                // update rightMax as no water can be trapped here
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    // Water can be trapped at this position
                    // The amount is the difference between rightMax and current height
                    result += rightMax - height[right];
                }
                // Move the right pointer to the left
                right--;
            }
        }

        // Return the total amount of water trapped
        return result;
    }

    // Test cases
    public static void main(String[] args) {

        // Test case 1: Standard case
        int[] height1 = { 0,1,0,2,1,0,1,3,2,1,2,1 };
        System.out.println("Test case 1: " + trap(height1)); // Expected output: 6

        // Test case 2: No water can be trapped
        int[] height2 = { 3,4,3 };
        System.out.println("Test case 2: " + trap(height2)); // Expected output: 0

        // Test case 3: Single peak
        int[] height3 = { 1,2,3,4,3,2,1 };
        System.out.println("Test case 3: " + trap(height3)); // Expected output: 0

        // Test case 4: Multiple peaks
        int[] height4 = { 3,0,2,0,4 };
        System.out.println("Test case 4: " + trap(height4)); // Expected output: 7

        // Test case 5: Empty array
        int[] height5 = {};
        System.out.println("Test case 5: " + trap(height5)); // Expected output: 0
    }
}

```





