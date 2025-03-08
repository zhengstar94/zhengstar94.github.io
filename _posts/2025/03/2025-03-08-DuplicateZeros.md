---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1089. Duplicate Zeros"
date: "2025-03-08"
tags: Easy TwoPointers
categories:
  - "LeetCode SingleSeqInPlacePointers" 
---

- Given a fixed-length integer array `arr`, duplicate each occurrence of zero, shifting the remaining elements to the right.
- **Note** that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

**Example 1**

```
Input: arr = [1,0,2,3,0,4,5,0]
Output: [1,0,0,2,3,0,0,4]
Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
```

**Example 2**

```
Input: arr = [1,2,3]
Output: [1,2,3]
Explanation: After calling your function, the input array is modified to: [1,2,3]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.SingleSeqInPlacePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/08
 */
public class DuplicateZeros {
    
    public static void duplicateZeros(int[] arr) {
        // Initialize array length and two pointers
        int n = arr.length;  // Length of input array
        int i = 0;          // Pointer for reading original array
        int j = 0;          // Pointer for tracking final positions after duplication

        // First phase: Calculate final positions
        // This loop simulates the duplication process to determine final positions
        while (j < n) {
            if (arr[i] == 0) {
                j++;    // When we find a zero, increment j an extra time (as zero will take two positions)
            }
            i++;       // Move reading pointer forward
            j++;       // Move position tracker forward
        }

        // Adjust pointers to last valid positions
        i--;    // Move back to last element we need to copy from
        j--;    // Move back to last position we need to copy to

        // Second phase: Actually copy elements from back to front
        while (i >= 0) {
            // If j is within array bounds, copy element from position i to position j
            if (j < n) {
                arr[j] = arr[i];
            }

            // If current element is zero and we have space for duplication
            // --j first decrements j then checks if it's >= 0
            if (arr[i] == 0 && --j >= 0) {
                arr[j] = 0;    // Place the duplicated zero
            }

            // Move both pointers backwards
            i--;    // Move reading pointer back
            j--;    // Move writing pointer back
        }
    }
    
    public static void main(String[] args) {
        // Test case 1: Array with zeros
        int[] arr1 = {1,0,2,3,0,4,5,0};
        System.out.print("Test case 1 before: ");
        printArray(arr1);
        duplicateZeros(arr1);
        System.out.print("Test case 1 after: ");
        printArray(arr1);

        // Test case 2: Array without zeros
        int[] arr2 = {1,2,3};
        System.out.print("Test case 2 before: ");
        printArray(arr2);
        duplicateZeros(arr2);
        System.out.print("Test case 2 after: ");
        printArray(arr2);
    }
    
    private static void printArray(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) {
                System.out.print(",");
            }
        }
        System.out.println("]");
    }
}

```