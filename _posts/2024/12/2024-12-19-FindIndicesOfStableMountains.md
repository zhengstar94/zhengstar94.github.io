---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3285. Find Indices of Stable Mountains"
date: "2024-12-19"
tags: Easy
categories:
  - "LeetCode Array"
---

- There are `n` mountains in a row, and each mountain has a height. You are given an integer array `height` where `height[i]` represents the height of mountain `i`, and an integer `threshold`.
- A mountain is called **stable** if the mountain just before it (**if it exists**) has a height **strictly greater** than `threshold`. **Note** that mountain 0 is **not** stable.
- Return an array containing the indices of *all* **stable** mountains in **any** order.


**Example 1**

```
Input: height = [1,2,3,4,5], threshold = 2

Output: [3,4]

Explanation:
Mountain 3 is stable because height[2] == 3 is greater than threshold == 2.
Mountain 4 is stable because height[3] == 4 is greater than threshold == 2.
```

**Example 2**

```
Input: height = [10,1,10,1,10], threshold = 3

Output: [1,3]
```

**Example 3**

```
Input: height = [10,1,10,1,10], threshold = 10

Output: []
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2024/12/19
 */
public class FindIndicesOfStableMountains {
    public static List<Integer> findIndicesOfStableMountains(int[] height, int threshold) {
        List<Integer> result = new ArrayList<>();

        // Start from index 1 since mountain 0 is never stable
        for (int i = 1; i < height.length; i++) {
            // Check if the height of the previous mountain is strictly greater than the threshold
            if (height[i - 1] > threshold) {
                result.add(i);
            }
        }

        return result;
    }

    // Main method containing test cases
    public static void main(String[] args) {

        // Test case 1
        int[] height1 = {1, 2, 3, 4, 5};
        int threshold1 = 2;
        System.out.println("Test case 1:");
        System.out.println("Input: height = " + Arrays.toString(height1) + ", threshold = " + threshold1);
        System.out.println("Output: " + findIndicesOfStableMountains(height1, threshold1));

        // Test case 2
        int[] height2 = {10, 1, 10, 1, 10};
        int threshold2 = 3;
        System.out.println("\nTest case 2:");
        System.out.println("Input: height = " + Arrays.toString(height2) + ", threshold = " + threshold2);
        System.out.println("Output: " + findIndicesOfStableMountains(height2, threshold2));

        // Test case 3
        int[] height3 = {10, 1, 10, 1, 10};
        int threshold3 = 10;
        System.out.println("\nTest case 3:");
        System.out.println("Input: height = " + Arrays.toString(height3) + ", threshold = " + threshold3);
        System.out.println("Output: " + findIndicesOfStableMountains(height3, threshold3));
    }
}
```





