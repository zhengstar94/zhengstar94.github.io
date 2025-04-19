---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1578. Minimum Time to Make Rope Colorful"
date: "2025-04-19"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---


- Alice has `n` balloons arranged on a rope. You are given a **0-indexed** string `colors` where `colors[i]` is the color of the `ith` balloon.
- Alice wants the rope to be **colorful**. She does not want **two consecutive balloons** to be of the same color, so she asks Bob for help. Bob can remove some balloons from the rope to make it **colorful**. You are given a **0-indexed** integer array `neededTime` where `neededTime[i]` is the time (in seconds) that Bob needs to remove the `ith` balloon from the rope.
- Return *the **minimum time** Bob needs to make the rope **colorful***.

**Example 1**

```
Input: colors = "abaac", neededTime = [1,2,3,4,5]
Output: 3
Explanation: In the above image, 'a' is blue, 'b' is red, and 'c' is green.
Bob can remove the blue balloon at index 2. This takes 3 seconds.
There are no longer two consecutive balloons of the same color. Total time = 3.
```

**Example 2**

```
Input: colors = "abc", neededTime = [1,2,3]
Output: 0
Explanation: The rope is already colorful. Bob does not need to remove any balloons from the rope.
```

**Example 3**

```
Input: colors = "aabaa", neededTime = [1,2,3,4,1]
Output: 2
Explanation: Bob will remove the balloons at indices 0 and 4. Each balloons takes 1 second to remove.
There are no longer two consecutive balloons of the same color. Total time = 1 + 1 = 2.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/19
 */
public class MinimumTimeToMakeRopeColorful {
    public static int minCost(String colors, int[] neededTime) {
        int n = colors.length();
        // Total time needed to remove balloons
        int totalTime = 0;
        // Index to traverse the array
        int i = 0;

        while (i < n) {
            // Get the current color of the balloon at position i
            char currentColor = colors.charAt(i);
            // Initialize maxTime with the time needed to remove current balloon
            // This will keep track of the maximum time among same-colored balloons
            int maxTime = neededTime[i];
            // Initialize sumTime with the time of current balloon
            // This will accumulate the total time of all same-colored balloons
            int sumTime = neededTime[i];
            // Move to next position after recording current balloon's information
            i++;

            // Keep checking next balloons as long as they have the same color
            while (i < n && colors.charAt(i) == currentColor) {
                // Update maxTime if current balloon takes longer to remove
                // We want to keep the balloon with maximum removal time
                maxTime = Math.max(maxTime, neededTime[i]);
                // Add current balloon's removal time to the total
                sumTime += neededTime[i];
                // Move to next position
                i++;
            }

            // If sumTime != maxTime, it means we found multiple balloons of same color
            // We need to remove all balloons except the one with maximum removal time
            // The cost of removal will be (sumTime - maxTime)
            // Example: if we have [1,2,3] of same color, maxTime=3, sumTime=6
            // We keep the balloon with time 3 and remove others, cost = 6-3 = 3
            if (sumTime != maxTime) {
                totalTime += (sumTime - maxTime);
            }
        }

        return totalTime;
    }

    public static void main(String[] args) {
        // Test case 1: Remove balloon with time 3 from "abaac"
        String colors1 = "abaac";
        int[] neededTime1 = {1, 2, 3, 4, 5};
        System.out.println("Test case 1: " + minCost(colors1, neededTime1)); // Should output 3

        // Test case 2: No removal needed as all colors are different
        String colors2 = "abc";
        int[] neededTime2 = {1, 2, 3};
        System.out.println("Test case 2: " + minCost(colors2, neededTime2)); // Should output 0

        // Test case 3: Remove balloons with times 1 and 1
        String colors3 = "aabaa";
        int[] neededTime3 = {1, 2, 3, 4, 1};
        System.out.println("Test case 3: " + minCost(colors3, neededTime3)); // Should output 2
    }
}

```





