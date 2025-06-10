---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2454. Next Greater Element IV"
date: "2025-06-10"
tags: Hard
categories:
  - "LeetCode Stack"
---


- You are given a **0-indexed** array of non-negative integers `nums`. For each integer in `nums`, you must find its respective **second greater** integer.
- The **second greater** integer of `nums[i]` is `nums[j]` such that:
  - `j > i`
  - `nums[j] > nums[i]`
  - There exists **exactly one** index `k` such that `nums[k] > nums[i]` and `i < k < j`.
- If there is no such `nums[j]`, the second greater integer is considered to be `-1`.
  - For example, in the array `[1, 2, 4, 3]`, the second greater integer of `1` is `4`, `2` is `3`, and that of `3` and `4` is `-1`.
- Return *an integer array* `answer`*, where* `answer[i]` *is the second greater integer of* `nums[i]`*.*

**Example 1**

```
Input: nums = [2,4,0,9,6]
Output: [9,6,6,-1,-1]
Explanation:
0th index: 4 is the first integer greater than 2, and 9 is the second integer greater than 2, to the right of 2.
1st index: 9 is the first, and 6 is the second integer greater than 4, to the right of 4.
2nd index: 9 is the first, and 6 is the second integer greater than 0, to the right of 0.
3rd index: There is no integer greater than 9 to its right, so the second greater integer is considered to be -1.
4th index: There is no integer greater than 6 to its right, so the second greater integer is considered to be -1.
Thus, we return [9,6,6,-1,-1].
```

**Example 2**

```
Input: nums = [3,3]
Output: [-1,-1]
Explanation:
We return [-1,-1] since neither integer has any integer greater than it.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.*;

/**
 * @Author zhengxingxing
 * @Date 2025/06/10
 */
public class NextGreaterElementIV {

    public static int[] secondGreaterElement(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];

        // Initialize all positions to -1 (meaning no second greater element found yet)
        Arrays.fill(result, -1);

        // stack1: stores indices of elements that are still waiting to find their FIRST greater element
        // These elements haven't found any greater element yet
        Deque<Integer> stack1 = new ArrayDeque<>();

        // stack2: stores indices of elements that have already found their FIRST greater element
        // and are now waiting to find their SECOND greater element
        Deque<Integer> stack2 = new ArrayDeque<>();

        // Process each element in the array from left to right
        for (int i = 0; i < n; i++) {

            // STEP 1: Process stack2 - try to find the SECOND greater element
            // For elements in stack2, they have already found their first greater element
            // Now we check if current element nums[i] can be their second greater element
            while (!stack2.isEmpty() && nums[stack2.peek()] < nums[i]) {
                int index = stack2.pop();  // Get the index from stack2
                result[index] = nums[i];   // Current element is the SECOND greater element for nums[index]
                // This index is now complete - it has found both first and second greater elements
            }

            // STEP 2: Collect elements from stack1 that found their FIRST greater element
            // We use a temporary list to store indices that will transition from stack1 to stack2
            List<Integer> temp = new ArrayList<>();

            // Process stack1 - try to find the FIRST greater element
            // For elements in stack1, they haven't found any greater element yet
            while (!stack1.isEmpty() && nums[stack1.peek()] < nums[i]) {
                // Current element nums[i] is the FIRST greater element for nums[stack1.peek()]
                temp.add(stack1.pop());  // Remove from stack1 and add to temp
                // These indices will now start looking for their SECOND greater element
            }

            // STEP 3: STATE TRANSITION - Move elements from "looking for first" to "looking for second"
            // Transfer the indices that just found their first greater element to stack2
            // CRITICAL: We insert in REVERSE ORDER to maintain monotonic property of stack2
            //
            // Why reverse order?
            // - Elements in temp are stored in the order they were popped from stack1 (larger elements first)
            // - To maintain monotonic increasing order in stack2 (from bottom to top)
            // - We need to reverse the insertion order
            //
            // Example: if temp = [idx_of_3, idx_of_2, idx_of_1] (popped in this order)
            // After reverse insertion: stack2 = [idx_of_1, idx_of_2, idx_of_3] (bottom to top)
            // This ensures stack2 maintains monotonic increasing property
            for (int j = temp.size() - 1; j >= 0; j--) {
                stack2.push(temp.get(j));
            }

            // STEP 4: Add current index to stack1
            // Current element starts its journey by looking for its FIRST greater element
            stack1.push(i);
        }

        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic example with mixed values
        int[] nums1 = {2, 4, 0, 9, 6};
        int[] result1 = secondGreaterElement(nums1);
        System.out.println("Test Case 1:");
        System.out.println("Input: " + Arrays.toString(nums1));
        System.out.println("Output: " + Arrays.toString(result1));
        System.out.println("Expected: [9, 6, 6, -1, -1]");
        System.out.println("Result: " + (Arrays.equals(result1, new int[]{9, 6, 6, -1, -1}) ? "PASS" : "FAIL"));
        System.out.println();

        // Test Case 2: All elements are equal
        int[] nums2 = {3, 3};
        int[] result2 = secondGreaterElement(nums2);
        System.out.println("Test Case 2:");
        System.out.println("Input: " + Arrays.toString(nums2));
        System.out.println("Output: " + Arrays.toString(result2));
        System.out.println("Expected: [-1, -1]");
        System.out.println("Result: " + (Arrays.equals(result2, new int[]{-1, -1}) ? "PASS" : "FAIL"));
        System.out.println();
    }
}
```





