---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "Group cycle"
date: "2025-04-07"
categories: 
  - "Data Structure"
giscus_comments: true
---

## Group cycle

### Core Concept

Group cycle is a powerful algorithmic technique specifically designed for handling problems that require dividing arrays or sequences into several groups and applying the same logical processing to each group. This pattern is particularly effective when dealing with data that has segmented characteristics, significantly simplifying code structure and improving readability.

When we face problems that require dividing continuous elements into different groups according to specific conditions, the group cycle pattern provides a clear solution. Unlike traditional single-layer loops, group cycle uses a double-layer loop structure, with outer and inner loops each having their own responsibilities:

1. **Outer Loop**: Responsible for two key tasks
   - Inter-group preparation: Recording the starting position of each group
   - Post-group statistics: Updating global results after processing a group (e.g., maximum values, counts, etc.)
2. **Inner Loop**: Focuses on single group processing
   - Determining the current group's boundary: Finding where the group ends
   - Applying intra-group processing logic: Executing required operations on the elements of the current group

The key advantage of this structure lies in its clear logic and well-defined boundaries, especially that it doesn't require special processing for the last group of data, which is often a common source of errors in coding.

### Example Problem

#### Problem Description

Given an integer array `nums` and an integer `threshold`, we need to find the longest subarray that satisfies the following conditions:

1. The first element of the subarray must be even
2. The parity of adjacent elements in the subarray must alternate (one odd, one even)
3. All elements in the subarray must not exceed the threshold We need to return the length of the longest subarray that satisfies these conditions.

**Example:**
- **Input:** nums = [3,2,5,4], threshold = 5
- **Output:** 3
- **Explanation:** The longest subarray that satisfies the conditions is [2,5,4], with a length of 3.

#### Algorithm Implementation
Using group cycle to solve this problem, we implement the following:

```java
public int longestAlternatingSubarray(int[] nums, int threshold) {
    int n = nums.length;
    int ans = 0, i = 0;
    while (i < n) {
        if (nums[i] > threshold || nums[i] % 2 != 0) {
            i++; // Skip elements that don't meet the initial conditions
            continue;
        }
        int start = i; // Record the starting position of this group
        i++; // Starting position already meets requirements, start judging from next position
        while (i < n && nums[i] <= threshold && nums[i] % 2 != nums[i - 1] % 2) {
            i++;
        }
        // From start to i-1 is a subarray that meets the requirements (and cannot be extended further)
        ans = Math.max(ans, i - start);
    }
    return ans;
}
```

#### Execution Step Analysis

Using the example input nums = [3,2,5,4], threshold = 5, the execution process is as follows:

1. **Initialization**: n = 4, ans = 0, i = 0
2. **First Outer Loop**:
   - nums[0] = 3, is odd, doesn't meet initial condition (must be even)
   - i++ → i = 1, continue to next iteration
3. **Second Outer Loop**:
   - nums[1] = 2, is even and ≤ threshold, meets initial condition
   - Record start = 1
   - i++ → i = 2, enter inner loop
   - Inner loop:
      - Check nums[2] = 5: ≤ threshold and has different parity from nums[1] (5 is odd, 2 is even)
      - Meets condition, i++ → i = 3
      - Check nums[3] = 4: ≤ threshold and has different parity from nums[2] (4 is even, 5 is odd)
      - Meets condition, i++ → i = 4
      - i = 4 is beyond array range, inner loop ends
   - Calculate subarray length: i - start = 4 - 1 = 3
   - Update ans = max(0, 3) = 3
4. **Loop End**: i = 4 ≥ n = 4, outer loop ends
5. **Return Result**: ans = 3

### Complexity Analysis

1. **Time Complexity: O(n)**
   - Although the code has nested loops, each element is visited at most once
   - The outer loop variable i doesn't simply increment, but jumps based on inner loop results
   - All elements are processed only once in total, so the time complexity is O(n)
2. **Space Complexity: O(1)**
   - Only uses a few variables (ans, i, start) to track state
   - No additional data structures related to input size are used

### Key Advantages

1. **Clear Logic**
   - Outer loop is responsible for finding suitable subarray starting points
   - Inner loop is responsible for extending the subarray until it can't be extended further
   - Maximum length is updated immediately after group processing is complete
2. **Concise Code**
   - No need for additional markers or complex condition judgments
   - Naturally handles subarray boundaries through the concept of groups
3. **Avoids Common Errors**
   - No need to specially process the last group of elements
   - Boundary conditions are built into the loop structure
4. **Completed in One Pass**
   - A single scan can find the answer, no need for repeated processing
   - Ensures algorithm efficiency


### Universal Pattern of Group Cycle
```java
void groupCycle(int[] nums) {
    int n = nums.length;
    int i = 0;
    
    while (i < n) {
        int start = i;
        
        // Inner loop: determine how far the current group can extend
        while (i < n && /* condition for continuing current group */) {
            i++;
        }
        
        // Elements from index 'start' to 'i - 1' form one group
        // You can process the group here, for example:
        // int groupLength = i - start;
        
        // No need to do i++ here — the index has already been advanced inside the inner loop
    }
}
```

The beauty of this pattern lies in the fact that the outer loop doesn't simply increment the index step by step—instead, it jumps forward based on the result of the inner loop. This ensures that each element is processed exactly once, while maintaining both clarity and efficiency in the code.

### Best Practices

1. **Clearly Define Group Boundary Conditions**
   - Clearly specify under what circumstances a new group starts
   - Clearly specify under what circumstances the current group ends
2. **Skip Elements That Don't Meet Conditions Early**
   - Check and skip elements that can't be group starting points in the outer loop
   - Reduce unnecessary calculations and checks
3. **Correctly Increment Loop Variables**
   - Distinguish between direct incrementation (i++) and condition-based incrementation
   - Increment loop variables at appropriate times to avoid missing or duplicate processing
4. **Correctly Calculate Group Length**
   - Group length is the end position minus the start position
   - Update the answer immediately after group processing is complete
5. **Reasonably Use the Continue Statement**
   - Use continue to skip conditions that don't meet requirements
   - Maintain the clarity of loop logic

### Application Scenarios

The group cycle technique is applicable to various algorithmic problems, especially those involving data with segmented characteristics:

1. **Processing Consecutive Identical Elements**
   - Calculate the longest sequence of consecutive identical characters
   - Compress consecutive repeated elements (e.g., AAABBC → 3A2B1C)
2. **Peak-Valley Analysis**
   - Find peaks and valleys in arrays
   - Analyze trend changes in time series data such as stock prices
3. **Interval Property Problems**
   - Find the longest/shortest intervals that satisfy specific conditions
   - Process sequences with alternating characteristics (such as odd-even alternating, up-down alternating)
4. **Pattern Recognition**
   - Identify specific patterns or regularities in sequences
   - Find substrings that satisfy specific rules in strings
5. **Sequence Segmentation Processing**
   - Divide sequences into multiple segments with similar properties
   - Apply different processing logic to different segments

### Key Considerations

When applying the group cycle technique, consider the following key factors:

1. **Group Definition**
   - Clearly define group start conditions: What kind of elements can serve as starting points for groups?
   - Clearly define group end conditions: Under what circumstances does the current group end?
   - These definitions directly determine the conditions and structure of the loop
2. **Handling Boundary Cases**
   - Empty array processing: The algorithm needs to correctly handle cases where the input is empty
   - Single element processing: Determine whether a valid group can be formed when the array has only one element
   - No satisfying groups exist: Ensure the algorithm returns an appropriate default value (such as 0)
3. **Index Management**
   - Index updates for inner and outer loops need to be correct, avoiding skipping elements or processing duplicates
   - Pay special attention to index handling after the group's starting position is confirmed
4. **Condition Optimization**
   - The order of condition judgments may affect performance, especially the application of short-circuit logic
   - In the inner loop, check conditions that are easier to fail first to end unnecessary calculations early
5. **Scalability**
   - Consider whether the algorithm is easy to extend to similar problems or more complex variants
   - Analysis of the universality and specificity of grouping conditions

### Summary

Group cycle is an efficient algorithmic technique for handling sequence segmentation problems. Its core idea is to divide sequences into multiple continuous groups through clearly defined inter-group and intra-group logic, and apply unified processing to each group. The main features of this technique include:

1. **Clear Structure**: The outer loop is responsible for inter-group management and result updates, while the inner loop focuses on single group processing and boundary determination, with clear responsibilities.
2. **High Efficiency**: Despite using nested loops, each element is processed at most once, ensuring linear time complexity.
3. **Concise Boundary Handling**: Naturally handles boundary conditions through the loop structure, avoiding common boundary errors.
4. **Wide Application Range**: From simple processing of consecutive identical elements to complex pattern recognition, group cycle provides elegant solutions.

