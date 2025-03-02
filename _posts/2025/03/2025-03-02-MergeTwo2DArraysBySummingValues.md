---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2570. Merge Two 2D Arrays by Summing Values"
date: "2025-03-02"
tags: Easy
categories:
  - "LeetCode Array" 
---

- You are given two **2D** integer arrays `nums1` and `nums2.`
  - `nums1[i] = [idi, vali]` indicate that the number with the id `idi` has a value equal to `vali`.
  - `nums2[i] = [idi, vali]` indicate that the number with the id `idi` has a value equal to `vali`.
- Each array contains **unique** ids and is sorted in **ascending** order by id.
- Merge the two arrays into one array that is sorted in ascending order by id, respecting the following conditions:
  - Only ids that appear in at least one of the two arrays should be included in the resulting array.
  - Each id should be included **only once** and its value should be the sum of the values of this id in the two arrays. If the id does not exist in one of the two arrays, then assume its value in that array to be `0`.
- Return *the resulting array*. The returned array must be sorted in ascending order by id.

**Example 1**

```
Input: nums1 = [ [1,2],[2,3],[4,5 ] ], nums2 = [ [1,4],[3,2],[4,1 ] ]
Output: [ [ 1,6],[2,3],[3,2],[4,6 ] ]
Explanation: The resulting array contains the following:
- id = 1, the value of this id is 2 + 4 = 6.
- id = 2, the value of this id is 3.
- id = 3, the value of this id is 2.
- id = 4, the value of this id is 5 + 1 = 6.
```

**Example 2**

```
Input: nums1 = [ [ 2,4],[3,6],[5,5 ] ], nums2 = [ [ 1,3],[4,3 ] ]
Output: [ [ 1,3],[2,4],[3,6],[4,3],[5,5 ] ]
Explanation: There are no common ids, so we just include each id with its value in the resulting list.
```

## Method 1

```tex
【O(n + m) time | O(n + m) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/03/02
 */
public class MergeTwo2DArraysBySummingValues {
    
    public static int[][] mergeArrays(int[][] nums1, int[][] nums2) {
        // List to store merged results
        List<int[]> result = new ArrayList<>();

        // Initialize pointers for both arrays
        int i = 0; // pointer for nums1
        int j = 0; // pointer for nums2

        // Process both arrays until we reach the end of either
        while (i < nums1.length && j < nums2.length) {
            // Case 1: ID in nums1 is smaller
            if (nums1[i][0] < nums2[j][0]) {
                result.add(nums1[i]); // Add element from nums1
                i++;
            }
            // Case 2: ID in nums2 is smaller
            else if (nums1[i][0] > nums2[j][0]) {
                result.add(nums2[j]); // Add element from nums2
                j++;
            }
            // Case 3: IDs are equal - sum the values
            else {
                // Create new array with same ID and summed values
                result.add(new int[]{nums1[i][0], nums1[i][1] + nums2[j][1]});
                i++;
                j++;
            }
        }

        // Add remaining elements from nums1 if any
        while (i < nums1.length) {
            result.add(nums1[i]);
            i++;
        }

        // Add remaining elements from nums2 if any
        while (j < nums2.length) {
            result.add(nums2[j]);
            j++;
        }

        // Convert ArrayList to 2D array and return
        return result.toArray(new int[result.size()][]);
    }

    /**
     * Main method with test cases
     * @param args Command line arguments (not used)
     */
    public static void main(String[] args) {
        // Test Case 1: Arrays with overlapping IDs
        int[][] nums1 = { { 1,2},{2,3},{4,5 } };
        int[][] nums2 = { { 1,4},{3,2},{4,1 } };
        System.out.println("Test Case 1:");
        printArray(mergeArrays(nums1, nums2));

        // Test Case 2: Arrays with non-overlapping IDs
        int[][] nums3 = { { 2,4},{3,6},{5,5 } };
        int[][] nums4 = { { 1,3},{4,3 } };
        System.out.println("\nTest Case 2:");
        printArray(mergeArrays(nums3, nums4));
    }

    /**
     * Helper method to print 2D array in readable format
     * @param arr 2D array to be printed
     */
    private static void printArray(int[][] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(Arrays.toString(arr[i]));
            if (i < arr.length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
    }
}

```





