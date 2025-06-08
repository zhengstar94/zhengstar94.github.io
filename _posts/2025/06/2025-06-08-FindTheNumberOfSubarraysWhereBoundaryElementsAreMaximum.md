---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3113. Find the Number of Subarrays Where Boundary Elements Are Maximum"
date: "2025-06-08"
tags: Hard
categories:
  - "LeetCode Stack"
---


- You are given an array of **positive** integers `nums`.
- Return the number of subarrays of `nums`, where the **first** and the **last** elements of the subarray are *equal* to the **largest** element in the subarray.

**Example 1**

```
Input: nums = [1,4,3,3,2]

Output: 6

Explanation:

There are 6 subarrays which have the first and the last elements equal to the largest element of the subarray:

subarray [1,4,3,3,2], with its largest element 1. The first element is 1 and the last element is also 1.
subarray [1,4,3,3,2], with its largest element 4. The first element is 4 and the last element is also 4.
subarray [1,4,3,3,2], with its largest element 3. The first element is 3 and the last element is also 3.
subarray [1,4,3,3,2], with its largest element 3. The first element is 3 and the last element is also 3.
subarray [1,4,3,3,2], with its largest element 2. The first element is 2 and the last element is also 2.
subarray [1,4,3,3,2], with its largest element 3. The first element is 3 and the last element is also 3.
Hence, we return 6.
```

**Example 2**

```
Input: nums = [3,3,3]

Output: 6

Explanation:

There are 6 subarrays which have the first and the last elements equal to the largest element of the subarray:

subarray [3,3,3], with its largest element 3. The first element is 3 and the last element is also 3.
subarray [3,3,3], with its largest element 3. The first element is 3 and the last element is also 3.
subarray [3,3,3], with its largest element 3. The first element is 3 and the last element is also 3.
subarray [3,3,3], with its largest element 3. The first element is 3 and the last element is also 3.
subarray [3,3,3], with its largest element 3. The first element is 3 and the last element is also 3.
subarray [3,3,3], with its largest element 3. The first element is 3 and the last element is also 3.
Hence, we return 6.
```

**Example 3**

```
Input: nums = [1]

Output: 1

Explanation:

There is a single subarray of nums which is [1], with its largest element 1. The first element is 1 and the last element is also 1.

Hence, we return 1.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @Author zhengxingxing
 * @Date 2025/06/08
 */
public class FindTheNumberOfSubarraysWhereBoundaryElementsAreMaximum {

    public static long numberOfSubarrays(int[] nums) {
        // Step 1: Initialize answer with array length
        // Every single element forms a valid subarray (first == last == max)
        long ans = nums.length;

        // Step 2: Initialize monotonic stack with sentinel value
        // Each stack element stores [value, count_of_occurrences]
        // Sentinel prevents empty stack checks and simplifies logic
        Deque<int[]> st = new ArrayDeque<>();
        st.push(new int[]{Integer.MAX_VALUE, 0}); // Sentinel: [∞, 0]

        // Step 3: Process each element as potential right boundary
        for (int x : nums) {

            // Phase 1: Maintain monotonic decreasing property
            // Remove all elements smaller than current element from stack
            // Why? Because if current element > stack_top, then stack_top cannot
            // be the maximum in any subarray that includes current element
            while (x > st.peek()[0]) {
                st.pop(); // Remove smaller elements
            }

            // Phase 2: Handle current element
            if (x == st.peek()[0]) {
                // Case 1: Current element equals stack top value
                // This means we found matching left boundaries for current right boundary
                // st.peek()[1] represents how many times this value appeared consecutively

                // Add contribution: each previous occurrence can pair with current element
                // to form a valid subarray (since all elements between them are <= x)
                ans += st.peek()[1];

                // Increment count for this value (current element becomes another occurrence)
                st.peek()[1]++;

            } else {
                // Case 2: Current element is smaller than stack top
                // Push current element as new potential left boundary
                // Start with count = 1 (this is the first occurrence at this "level")
                st.push(new int[]{x, 1});
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Mixed values
        int[] nums1 = {1, 4, 3, 3, 2};
        System.out.println("=== Test Case 1: [1,4,3,3,2] ===");
        System.out.println("Algorithm Result: " + numberOfSubarrays(nums1));
        System.out.println("Expected Result: 6");
        System.out.println("Valid subarrays: [1], [4], [3], [3], [2], [3,3]");
        System.out.println();

        // Test case 2: All same elements
        int[] nums2 = {3, 3, 3};
        System.out.println("=== Test Case 2: [3,3,3] ===");
        System.out.println("Algorithm Result: " + numberOfSubarrays(nums2));
        System.out.println("Expected Result: 6");
        System.out.println("Valid subarrays: [3], [3], [3], [3,3], [3,3], [3,3,3]");
        System.out.println();

        // Test case 3: Single element
        int[] nums3 = {1};
        System.out.println("=== Test Case 3: [1] ===");
        System.out.println("Algorithm Result: " + numberOfSubarrays(nums3));
        System.out.println("Expected Result: 1");
        System.out.println("Valid subarrays: [1]");
        System.out.println();

        // Test case 4: Strictly increasing array
        int[] nums4 = {1, 2, 3, 4, 5};
        System.out.println("=== Test Case 4: [1,2,3,4,5] ===");
        System.out.println("Algorithm Result: " + numberOfSubarrays(nums4));
        System.out.println("Expected Result: 5");
        System.out.println("Valid subarrays: Only single elements [1], [2], [3], [4], [5]");
        System.out.println();

        // Test case 5: All identical elements
        int[] nums5 = {2, 2, 2, 2};
        System.out.println("=== Test Case 5: [2,2,2,2] ===");
        System.out.println("Algorithm Result: " + numberOfSubarrays(nums5));
        System.out.println("Expected Result: 10");
        System.out.println("Calculation: 4 single + 3 pairs + 2 triplets + 1 quadruple = 10");
        System.out.println();
    }
}
```





