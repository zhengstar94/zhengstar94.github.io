---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1847. Closest Room"
date: "2024-12-16"
tags: Hard
categories:
  - "LeetCode TwoPointers"
---

- There is a hotel with `n` rooms. The rooms are represented by a 2D integer array `rooms` where `rooms[i] = [roomIdi, sizei]` denotes that there is a room with room number `roomIdi` and size equal to `sizei`. Each `roomIdi` is guaranteed to be **unique**.
- You are also given `k` queries in a 2D array `queries` where `queries[j] = [preferredj, minSizej]`. The answer to the `jth` query is the room number `id` of a room such that:
  - The room has a size of **at least** `minSizej`, and
  - `abs(id - preferredj)` is **minimized**, where `abs(x)` is the absolute value of `x`.
- If there is a **tie** in the absolute difference, then use the room with the **smallest** such `id`. If there is **no such room**, the answer is `-1`.
- Return *an array* `answer` *of length* `k` *where* `answer[j]` *contains the answer to the* `jth` *query*.


**Example 1**

```
Input: rooms = [ [ 2,2],[1,2],[3,2 ] ], queries = [ [ 3,1],[3,3],[5,2 ] ]
Output: [3,-1,3]
Explanation: The answers to the queries are as follows:
Query = [3,1]: Room number 3 is the closest as abs(3 - 3) = 0, and its size of 2 is at least 1. The answer is 3.
Query = [3,3]: There are no rooms with a size of at least 3, so the answer is -1.
Query = [5,2]: Room number 3 is the closest as abs(3 - 5) = 2, and its size of 2 is at least 2. The answer is 3.
```

**Example 2**

```
Input: rooms = [ [ 1,4],[2,3],[3,5],[4,1],[5,2 ] ], queries = [ [ 2,3],[2,4],[2,5 ] ]
Output: [2,1,3]
Explanation: The answers to the queries are as follows:
Query = [2,3]: Room number 2 is the closest as abs(2 - 2) = 0, and its size of 3 is at least 3. The answer is 2.
Query = [2,4]: Room numbers 1 and 3 both have sizes of at least 4. The answer is 1 since it is smaller.
Query = [2,5]: Room number 3 is the only room with a size of at least 5. The answer is 3.
```

## Method 1

```tex
【O(NlogN + QlogQ + QlogN) time | O(N + Q) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;
import java.util.TreeSet;

/**
 * @author zhengxingxing
 * @date 2024/12/16
 */
public class ClosestRoom {
    public static int[] closestRoom(int[][] rooms, int[][] queries) {
        // Create an array to track original query indices
        // This allows us to maintain the original query order while sorting
        Integer[] queryIndices = new Integer[queries.length];
        for (int i = 0; i < queries.length; i++) {
            queryIndices[i] = i;  // Initialize with original indices
        }

        // Sort query indices based on room size in descending order
        // This helps process larger room size requirements first
        Arrays.sort(queryIndices, (a, b) -> Integer.compare(queries[b][1], queries[a][1]));

        // Sort rooms by size in descending order
        // Allows efficient filtering of rooms that meet size requirements
        Arrays.sort(rooms, (a, b) -> Integer.compare(b[1], a[1]));

        // Initialize result array and room ID set
        int[] answer = new int[queries.length];
        TreeSet<Integer> roomIds = new TreeSet<>();

        // Track which rooms have been processed
        int roomIndex = 0;

        // Process each query in sorted order
        for (int queryIdx : queryIndices) {
            // Extract query parameters
            int preferred = queries[queryIdx][0];  // Preferred room number
            int minSize = queries[queryIdx][1];    // Minimum room size

            // Add room IDs that meet the size requirement
            // Dynamically build a set of valid room IDs
            while (roomIndex < rooms.length && rooms[roomIndex][1] >= minSize) {
                roomIds.add(rooms[roomIndex][0]);
                roomIndex++;
            }

            // If no rooms meet the size requirement, mark as -1
            if (roomIds.isEmpty()) {
                answer[queryIdx] = -1;
                continue;
            }

            // Find the closest room numbers
            // floor: largest room number <= preferred
            // ceiling: smallest room number >= preferred
            Integer lower = roomIds.floor(preferred);
            Integer higher = roomIds.ceiling(preferred);

            // Handle edge cases where lower or higher might be null
            if (lower == null) {
                // No room number lower than preferred, use higher
                answer[queryIdx] = higher;
            } else if (higher == null) {
                // No room number higher than preferred, use lower
                answer[queryIdx] = lower;
            } else {
                // Compare absolute differences to find the closest room
                int lowDiff = Math.abs(preferred - lower);
                int highDiff = Math.abs(preferred - higher);

                // Choose the closer room
                // If distances are equal, prefer the lower room number
                answer[queryIdx] = (lowDiff <= highDiff) ? lower : higher;
            }
        }

        return answer;
    }

    public static void main(String[] args) {

        // Test Case 1: Basic scenario
        int[][] rooms1 = { { 2,2}, {1,2}, {3,2 } };
        int[][] queries1 = { { 3,1}, {3,3}, {5,2 } };
        int[] result1 = closestRoom(rooms1, queries1);
        System.out.println("Test Case 1 Result: " + Arrays.toString(result1));
        // Expected: [3, -1, 3]

        // Test Case 2: More complex scenario
        int[][] rooms2 = { { 1,4}, {2,3}, {3,5}, {4,1}, {5,2 } };
        int[][] queries2 = { { 2,3}, {2,4}, {2,5 } };
        int[] result2 = closestRoom(rooms2, queries2);
        System.out.println("Test Case 2 Result: " + Arrays.toString(result2));
        // Expected: [2, 1, 3]

        // Test Case 3: Edge cases
        int[][] rooms3 = { { 1,10}, {2,20}, {3,30 } };
        int[][] queries3 = { { 1,9}, {2,21}, {3,40 } };
        int[] result3 = closestRoom(rooms3, queries3);
        System.out.println("Test Case 3 Result: " + Arrays.toString(result3));
        // Expected: [1, 3, -1]
    }
}
```
