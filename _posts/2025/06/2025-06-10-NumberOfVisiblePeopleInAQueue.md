---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1944. Number of Visible People in a Queue"
date: "2025-06-10"
tags: Hard
categories:
  - "LeetCode Stack"
---

- There are `n` people standing in a queue, and they numbered from `0` to `n - 1` in **left to right** order. You are given an array `heights` of **distinct** integers where `heights[i]` represents the height of the `ith` person.
- A person can **see** another person to their right in the queue if everybody in between is **shorter** than both of them. More formally, the `ith` person can see the `jth` person if `i < j` and `min(heights[i], heights[j]) > max(heights[i+1], heights[i+2], ..., heights[j-1])`.
- Return *an array* `answer` *of length* `n` *where* `answer[i]` *is the **number of people** the* `ith` *person can **see** to their right in the queue*.

**Example 1**

```
Input: heights = [10,6,8,5,11,9]
Output: [3,1,2,1,1,0]
Explanation:
Person 0 can see person 1, 2, and 4.
Person 1 can see person 2.
Person 2 can see person 3 and 4.
Person 3 can see person 4.
Person 4 can see person 5.
Person 5 can see no one since nobody is to the right of them.
```

**Example 2**

```
Input: heights = [5,1,2,3,10]
Output: [4,1,1,1,0]
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Deque;

/**
 * @Author zhengxingxing
 * @Date 2025/06/10
 */
public class NumberOfVisiblePeopleInAQueue {


    public static int[] canSeePersonsCount(int[] heights) {
        // Get the total number of people in the queue
        int n = heights.length;

        // Initialize result array to store the count for each person
        // result[i] will contain the number of people person i can see
        int[] result = new int[n];

        // Create a monotonic decreasing stack using ArrayDeque
        // The stack stores INDICES (not heights) of people
        // Stack property: heights[stack.bottom] >= heights[stack.top]
        // This means from bottom to top, the heights are in decreasing order
        Deque<Integer> stack = new ArrayDeque<>();

        // CRITICAL: Traverse from RIGHT to LEFT (i.e., from last person to first person)
        // Why backwards? Because we need to know about people on the right side first
        // before we can determine how many people the current person can see
        for (int i = n - 1; i >= 0; i--) {

            // Initialize count for current person at position i
            // This will store how many people person i can see to their right
            int count = 0;

            // PHASE 1: Count people shorter than current person
            // These shorter people will be "blocked" by the current person
            // We need to remove them from stack because they become invisible
            // to people on the left side of current person
            while (!stack.isEmpty() && heights[stack.peek()] < heights[i]) {
                // Remove the shorter person from stack (they get blocked)
                stack.pop();

                // Increment count because current person can see this shorter person
                count++;
            }

            // PHASE 2: Count the first person taller than or equal to current person
            // After the while loop, if stack is not empty, the top element represents
            // a person who is taller than or equal to the current person
            // The current person can see this taller person, but cannot see beyond them
            // because this taller person will block the view to anyone behind them
            if (!stack.isEmpty()) {
                // There's a taller person that current person can see
                count++;
            }

            // Store the total count of people that person i can see
            result[i] = count;

            // Add current person's index to the stack
            // This person might be visible to people on their left side
            stack.push(i);
        }

        // Return the result array containing counts for all people
        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Mixed heights with multiple visibility scenarios
        // Expected behavior:
        // Person 0 (height 10): can see persons 1(6), 2(8), 4(11) = 3 people
        // Person 1 (height 6): can see person 2(8) = 1 person
        // Person 2 (height 8): can see persons 3(5), 4(11) = 2 people  
        // Person 3 (height 5): can see person 4(11) = 1 person
        // Person 4 (height 11): can see person 5(9) = 1 person
        // Person 5 (height 9): no one to the right = 0 people
        int[] heights1 = {10, 6, 8, 5, 11, 9};
        int[] result1 = canSeePersonsCount(heights1);
        System.out.println("Test Case 1:");
        System.out.println("Input: " + Arrays.toString(heights1));
        System.out.println("Output: " + Arrays.toString(result1));
        System.out.println("Expected: [3, 1, 2, 1, 1, 0]");
        System.out.println("Result: " + (Arrays.equals(result1, new int[]{3, 1, 2, 1, 1, 0}) ? "PASS" : "FAIL"));
        System.out.println();

        // Test Case 2: Scenario where first person can see everyone
        // Expected behavior:
        // Person 0 (height 5): can see all 4 people to the right = 4 people
        // Person 1 (height 1): can see person 2(2) = 1 person
        // Person 2 (height 2): can see person 3(3) = 1 person
        // Person 3 (height 3): can see person 4(10) = 1 person  
        // Person 4 (height 10): no one to the right = 0 people
        int[] heights2 = {5, 1, 2, 3, 10};
        int[] result2 = canSeePersonsCount(heights2);
        System.out.println("Test Case 2:");
        System.out.println("Input: " + Arrays.toString(heights2));
        System.out.println("Output: " + Arrays.toString(result2));
        System.out.println("Expected: [4, 1, 1, 1, 0]");
        System.out.println("Result: " + (Arrays.equals(result2, new int[]{4, 1, 1, 1, 0}) ? "PASS" : "FAIL"));
        System.out.println();
    }
}
```





