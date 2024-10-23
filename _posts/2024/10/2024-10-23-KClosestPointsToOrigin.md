---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "973.K Closest Points to Origin"
date: "2024-10-23"
categories:
  - "LeetCode Array"
---


- Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.
- The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)^2 + (y1 - y2)^2`).
- You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).


**Example 1**

```
Input: points = [ [1,3],[-2,2] ], k = 1
Output: [ [-2,2] ]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [ [-2,2] ].
```

**Example 2**

```
Input: points = [ [3,3],[5,-1],[-2,4] ], k = 2
Output: [ [3,3],[-2,4] ]
Explanation: The answer [ [-2,4],[3,3] ] would also be accepted.
```

## Method 1

```tex
【O(n^2) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/10/23
 */
public class KClosestPointsToOrigin {
    public static int[][] kClosest(int[][] points, int k) {
        if(k ==  points.length){
            return points;
        }

        quickSelect(points, 0, points.length - 1, k);
        return Arrays.copyOf(points, k);
    }

    private static void quickSelect(int[][] points, int left, int right, int k) {
        // Base case: if left >= right, partition contains 1 or 0 elements
        if(left >= right){
            return;
        }

        // Choose random pivot to avoid worst case scenario
        int pivotIndex = left + (int)(Math.random() * (right - left + 1));
        // Partition array around pivot and get final position of pivot
        pivotIndex = partition(points, left, right, pivotIndex);

        // If pivot is at k-1, we've found our k elements
        if(pivotIndex == k -1){
            return;
        }
        // If pivot index is less than k-1, search right partition
        else if (pivotIndex < k - 1) {
            quickSelect(points, pivotIndex + 1, right, k);
        }
        // If pivot index is greater than k-1, search left partition
        else {
            quickSelect(points, left, pivotIndex - 1, k);
        }
    }

    /**
     * Implementation steps:
     * 1. SWAP 1: Move pivot to end (temporary storage)
     *    - swap(points, pivotIndex, right)
     * 2. Partition array:
     *    - SWAP 2: For each point with distance < pivot distance
     *      swap(points, current, storeIndex)
     * 3. SWAP 3: Move pivot to final position
     *    - swap(points, storeIndex, right)
     */
    private static int partition(int[][] points, int left, int right, int pivotIndex) {
        // Calculate distance of pivot point from origin
        int pivotDist = distance(points[pivotIndex]);

        // Move pivot to end of array
        swap(points, pivotIndex, right);
        int storeIndex = left;

        // Partition array around pivot distance
        // Move all points with distance less than pivot distance to left side
        for (int i = left; i < right; i++) {
            if(distance(points[i]) < pivotDist){
                swap(points, i, storeIndex);
                storeIndex++;
            }
        }

        // Move pivot to its final position
        swap(points, storeIndex, right);
        return storeIndex;
    }

    private static int distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }

    private static void swap(int[][] points, int i, int j) {
        int[] temp = points[i];
        points[i] = points[j];
        points[j] = temp;
    }

    public static void main(String[] args) {
        // Test Case 1: Normal case with multiple points
        int[][] points1 = { {1,3},{-2,2},{2,-2} };
        int k1 = 2;
        System.out.println(Arrays.deepToString(kClosest(points1, k1)));

        // Test Case 2: When k equals array length
        int[][] points2 = { {3,3},{5,-1},{-2,4} };
        int k2 = 3;
        System.out.println(Arrays.deepToString(kClosest(points2, k2)));

        // Test Case 3: Single point case
        int[][] points3 = { {1,1} };
        int k3 = 1;
        System.out.println(Arrays.deepToString(kClosest(points3, k3)));

        // Test Case 4: Case with origin point
        int[][] points4 = { {1,1},{-1,-1},{0,0},{2,2} };
        int k4 = 2;
        System.out.println(Arrays.deepToString(kClosest(points4, k4)));
    }
}

```





