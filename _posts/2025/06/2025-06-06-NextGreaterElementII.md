---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "503. Next Greater Element II"
date: "2025-06-06"
tags: Medium
categories:
  - "LeetCode Stack"
---


- Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return *the **next greater number** for every element in* `nums`.
- The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

**Example 1**

```
Input: nums = [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.
```

**Example 2**

```
Input: nums = [1,2,3,4,3]
Output: [2,3,4,-1,4]
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Stack;

import java.util.Arrays;
import java.util.Stack;

/**
 * @Author zhengxingxing
 * @Date 2025/06/06
 */
public class NextGreaterElementII {
    
    public static int[] nextGreaterElements(int[] nums) {
        // Get the length of input array
        int n = nums.length;

        // Initialize result array with -1 (default value when no greater element found)
        // This handles the case where some elements don't have a next greater element
        int[] result = new int[n];
        Arrays.fill(result, -1);  // Fill all positions with -1 initially

        // Monotonic decreasing stack to store array indices
        // Stack property: elements corresponding to indices in stack are in decreasing order
        // from bottom to top. This allows us to efficiently find next greater elements.
        Stack<Integer> stack = new Stack<>();

        // CORE ALGORITHM: Traverse the array twice to simulate circular array behavior
        // First traversal (i = 0 to n-1): Handle normal cases and build the stack
        // Second traversal (i = n to 2n-1): Handle circular cases using the built stack
        for (int i = 0; i < 2 * n; i++) {
            // Calculate actual array index using modulo operation
            // This maps: 0,1,2,3,4,5,6,7,8,9 -> 0,1,2,3,4,0,1,2,3,4 for n=5
            int currentIndex = i % n;

            // CRITICAL SECTION: Process stack while current element is greater than stack top
            // This is where we find the "next greater element" for previously seen smaller elements
            while (!stack.isEmpty() && nums[currentIndex] > nums[stack.peek()]) {
                // Pop the index from stack - this element has found its next greater element
                int index = stack.pop();

                // Set the result: nums[currentIndex] is the next greater element for nums[index]
                // This works because:
                // 1. All elements between index and currentIndex in the original traversal were smaller
                // 2. nums[currentIndex] is the first element greater than nums[index]
                result[index] = nums[currentIndex];
            }

            // STACK BUILDING PHASE: Only push indices during first traversal
            // Second traversal is purely for finding answers, not for building stack
            if (i < n) {
                // Push current index to stack
                // This element is waiting to find its next greater element
                stack.push(currentIndex);
            }
            // Note: During second traversal (i >= n), we don't push to stack
            // We only use existing stack elements to find circular next greater elements
        }

        // After both traversals, elements still in stack don't have next greater elements
        // Their result remains -1 (initialized value)
        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic circular case
        // nums[2] = 1 needs to look circularly to find next greater element nums[1] = 2
        int[] nums1 = {1, 2, 1};
        int[] result1 = nextGreaterElements(nums1);
        System.out.println("Test Case 1:");
        System.out.println("Input: " + Arrays.toString(nums1));
        System.out.println("Output: " + Arrays.toString(result1));
        System.out.println("Expected: [2, -1, 2]");
        System.out.println();

        // Test Case 2: Mixed case with circular element
        // nums[4] = 3 finds next greater element nums[3] = 4 by going circularly
        int[] nums2 = {1, 2, 3, 4, 3};
        int[] result2 = nextGreaterElements(nums2);
        System.out.println("Test Case 2:");
        System.out.println("Input: " + Arrays.toString(nums2));
        System.out.println("Output: " + Arrays.toString(result2));
        System.out.println("Expected: [2, 3, 4, -1, 4]");
        System.out.println();

        // Test Case 3: All elements are equal
        // No element has a next greater element
        int[] nums3 = {5, 5, 5};
        int[] result3 = nextGreaterElements(nums3);
        System.out.println("Test Case 3:");
        System.out.println("Input: " + Arrays.toString(nums3));
        System.out.println("Output: " + Arrays.toString(result3));
        System.out.println("Expected: [-1, -1, -1]");
        System.out.println();

        // Test Case 4: Single element
        // Single element cannot have next greater element
        int[] nums4 = {1};
        int[] result4 = nextGreaterElements(nums4);
        System.out.println("Test Case 4:");
        System.out.println("Input: " + Arrays.toString(nums4));
        System.out.println("Output: " + Arrays.toString(result4));
        System.out.println("Expected: [-1]");
        System.out.println();

        // Test Case 5: Strictly decreasing array
        // Each element (except first) finds next greater element by going circularly to the first element
        int[] nums5 = {5, 4, 3, 2, 1};
        int[] result5 = nextGreaterElements(nums5);
        System.out.println("Test Case 5:");
        System.out.println("Input: " + Arrays.toString(nums5));
        System.out.println("Output: " + Arrays.toString(result5));
        System.out.println("Expected: [-1, 5, 5, 5, 5]");
    }
}
```





