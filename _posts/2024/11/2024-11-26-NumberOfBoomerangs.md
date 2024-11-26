---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "447. Number of Boomerangs"
date: "2024-11-26"
categories:
  - "LeetCode HashTable"
---

- You are given `n` `points` in the plane that are all **distinct**, where `points[i] = [xi, yi]`. A **boomerang** is a tuple of points `(i, j, k)` such that the distance between `i` and `j` equals the distance between `i` and `k` **(the order of the tuple matters)**.
- Return *the number of boomerangs*.

**Example 1**

```
Input: points = [ [0,0],[1,0],[2,0] ]
Output: 2
Explanation: The two boomerangs are [ [1,0],[0,0],[2,0] ] and [ [1,0],[2,0],[0,0] ].
```

**Example 2**

```
Input: points = [ [1,1],[2,2],[3,3] ]
Output: 2
```

**Example 3**

```
Input: points = [ [ 1,1 ] ]
Output: 0
```

## Method 1

```tex
【O(n^2) time | O(n) space】
```

```java
package Leetcode.HashTable;
import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/11/26
 */
public class NumberOfBoomerangs {

    public static int numberOfBoomerangs(int[][] points) {
        // Variable to store the total boomerang count
        int result = 0;

        // Iterate through each point as the potential central point
        for (int i = 0; i < points.length; i++) {
            // HashMap to store distances and their point frequencies
            Map<Integer, Integer> distanceMap = new HashMap<>();

            // Calculate distances from current point to all other points
            for (int j = 0; j < points.length; j++) {
                // Skip calculating distance to the same point
                if (i != j) {
                    // Calculate squared distance between points
                    int distance = calculateDistance(points[i], points[j]);

                    // Update frequency of this distance in the map
                    // If distance not exists, set count to 1; otherwise, increment
                    distanceMap.put(distance, distanceMap.getOrDefault(distance, 0) + 1);
                }
            }

            // Calculate boomerang combinations for each distance group
            for (int count : distanceMap.values()) {
                /*
                 * Boomerang Calculation Logic:
                 * - 'count' represents points at the same distance from central point
                 * - For each distance group, calculate possible boomerang arrangements
                 *
                 * Mathematical Combination Explanation:
                 * 1. Total points at same distance: count
                 * 2. Choose first point: count options
                 * 3. Choose second point: (count - 1) options
                 * 4. Calculation: count * (count - 1)
                 *
                 * Example:
                 * If 4 points are at same distance:
                 * - First point selection: 4 ways
                 * - Second point selection: 3 ways
                 * - Total boomerang arrangements: 4 * 3 = 12
                 */
                result += count * (count - 1);
            }
        }

        return result;
    }
    
    private static int calculateDistance(int[] point1, int[] point2) {
        // Calculate x and y coordinate differences
        int dx = point1[0] - point2[0];
        int dy = point1[1] - point2[1];

        // Return squared distance (Pythagorean theorem without square root)
        return dx * dx + dy * dy;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Points in a diagonal line
        // Expected to have some boomerang arrangements
        int[][] points1 = { { 1,1},{2,2},{3,3 } };
        System.out.println("Test Case 1 Result: " + numberOfBoomerangs(points1));

        // Test Case 2: Points in a horizontal line
        // High likelihood of boomerang formations
        int[][] points2 = { { 0,0},{1,0},{2,0 } };
        System.out.println("Test Case 2 Result: " + numberOfBoomerangs(points2));

        // Test Case 3: Single point
        // No boomerangs possible
        int[][] points3 = { { 1,1 } };
        System.out.println("Test Case 3 Result: " + numberOfBoomerangs(points3));
    }
}
```

