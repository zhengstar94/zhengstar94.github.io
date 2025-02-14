---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "Sliding Window Techniques"
date: "2025-01-12"
categories: 
  - "Data Structure"
---


## Comparison of Fixed-Length and Dynamic-Length Windows

| Type    | Window Length | Typical Problems                                             | Core Operations                                             |
| ------- | ------------- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| Fixed   | Constant      | Fixed-length substring statistics, K-size subarray calculations | Element enters and exits immediately to maintain fixed size |
| Dynamic | Variable      | Longest/shortest substring search, Condition-based subarray finding | Window contracts only when conditions are violated          |

-----

[Previous content remains exactly the same, starting with "Part 1: Master Fixed-Length Sliding Window - A Universal Approach" through the end of Part 2]

-----

## Part 1: Master Fixed-Length Sliding Window - A Universal Approach

### Core Concept

We aim to calculate the maximum number of vowels in any substring with a length of exactly k. While brute-forcing all substrings results in a time complexity of O(nk), this approach is too slow. Can we achieve O(1) substring property calculations? Yes!

For instance, in the string "abci", if we already know the vowel count in substring "abc", then to compute it for "bci":

1. Check if the leaving character ('a') is a vowel
2. Check if the entering character ('i') is a vowel

This works because the middle characters ('b' and 'c') remain unchanged in both substrings.

### Example Walkthrough

**Input:** s = "abciiidef", k = 3

**Step-by-step:**

1. Traverse s from left to right
2. Count vowels in the first k-1 = 2 characters. Initially, there is 1 vowel
3. Start processing the sliding window:
   - s[2] = 'c' enters window, forming "abc" (1 vowel). Update max count. Then s[0] = 'a' exits window, reducing count to 0
   - s[3] = 'i' enters window, forming "bci" (1 vowel). Update max count. Then s[1] = 'b' exits, keeping count at 1
   - s[4] = 'i' enters window, forming "cii" (2 vowels). Update max count. Then s[2] = 'c' exits, keeping count at 2
   - s[5] = 'i' enters window, forming "iii" (3 vowels). Update max count. Then s[3] = 'i' exits, reducing count to 2
   - s[6] = 'd' enters window, forming "iid" (2 vowels). Update max count. Then s[4] = 'i' exits, reducing count to 1
   - s[7] = 'e' enters window, forming "ide" (2 vowels). Update max count. Then s[5] = 'i' exits, reducing count to 1
   - s[8] = 'f' enters window, forming "def" (1 vowel). Update max count. Traversal complete

### Fixed-Length Sliding Window Pattern

This pattern follows three simple steps: **Enter-Update-Exit**

1. **Enter**: Element at index i enters the window; update statistics. Repeat if i < k-1
2. **Update**: Update the answer (usually max/min value)
3. **Exit**: Element at index i-k+1 exits the window; update statistics

This pattern is universally applicable to all fixed-length sliding window problems.

### Implementation

```java
class Solution {
    public int maxVowels(String S, int k) {
        char[] s = S.toCharArray();
        int ans = 0;
        int vowel = 0;
        for (int i = 0; i < s.length; i++) {
            // 1. Enter window
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' ||
                s[i] == 'o' || s[i] == 'u') {
                vowel++;
            }
            if (i < k - 1) { // Window size less than k
                continue;
            }
            // 2. Update answer
            ans = Math.max(ans, vowel);
            // 3. Exit window
            char out = s[i - k + 1];
            if (out == 'a' || out == 'e' || out == 'i' ||
                out == 'o' || out == 'u') {
                vowel--;
            }
        }
        return ans;
    }
}
```

### Complexity Analysis

- **Time Complexity:** O(n), where n is the length of s
- **Space Complexity:** O(1), using only a few extra variables

### Key Benefits

1. Universal applicability to fixed-length sliding window problems
2. Simple and easy-to-remember three-step process: **Enter-Update-Exit**
3. Efficient O(n) time complexity
4. Minimal space usage (O(1))

### Best Practices

1. Initialize your window with the first k-1 elements
2. Update the answer after forming the complete window
3. Update statistics for both entering and exiting elements
4. Handle boundary conditions carefully

### Applications

This pattern can be adapted for various fixed-length sliding window problems, such as:

- Finding the maximum/minimum sum of k consecutive elements
- Finding the maximum/minimum average of k consecutive elements
- Counting occurrences of specific patterns in k-length windows
- Calculating statistics over k-length sliding windows

-----

## Part 2: Dynamic-Length Sliding Window

### Core Concept

The sliding window is a powerful and efficient algorithmic technique for solving problems involving subarrays or substrings. It is particularly useful for problems requiring dynamic adjustments to window size. With sliding window techniques, we reduce time complexity from O(n²) to O(n) by avoiding unnecessary recalculations.

This technique helps efficiently compute subarray properties by dynamically adjusting window boundaries (start and end points). The general process involves three key steps:

1. **Window Expansion**: Expand the window by moving the right boundary while updating required properties
2. **Condition Validation**: After every expansion, check whether the current window satisfies the required conditions
3. **Window Contraction**: If the condition is violated, shrink the window from the left while recalculating the required properties

### Example Problem

**Problem:**
Given a binary array nums containing only 0 and 1, delete exactly one element, and return the length of the longest subarray that consists only of 1.

**Input**: nums = [1, 1, 0, 1]
**Output**: 3

### Dynamic Sliding Window Steps

For this problem, we apply the sliding window pattern as follows:

1. **Enter Window**: Track the count of zeros as we expand the window
2. **Validate Condition**: Ensure we don't have more than one zero in our window
3. **Exit Window**: Shrink the window when we exceed our zero limit

### Implementation

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int zeroCount = 0;
        int maxLength = 0;
        int start = 0;

        for (int end = 0; end < nums.length; end++) {
            // 1. Expand window
            if (nums[end] == 0) {
                zeroCount++;
            }

            // 2. Contract window
            while (zeroCount > 1) {
                if (nums[start] == 0) {
                    zeroCount--;
                }
                start++;
            }

            // 3. Update result
            maxLength = Math.max(maxLength, end - start);
        }

        // Handle edge case: all ones array
        return maxLength == nums.length ? maxLength - 1 : maxLength;
    }
}
```

### Explanation of Key Steps

1. **Window Expansion**: Add the current element to the window and update zero count
2. **Window Contraction**: When zero count exceeds 1, shrink window from left while reducing zero count
3. **Update Result**: Continuously calculate valid window size and track maximum length

### Complexity Analysis

1. **Time Complexity**:
   - O(n): Each element is processed at most twice—once when entering the window and once when exiting
2. **Space Complexity**:
   - O(1): Only a few extra variables are used to track the window's start and end, with no additional data structures

### Applications and Scenarios

Dynamic sliding windows are highly versatile and applicable across various scenarios:

1. **Longest Subarray Problems**:
   - Example: Finding the longest substring with at most K distinct characters
2. **Frequency or Condition Checks**:
   - Example: Tracking the frequency of a specific element within a window
3. **Subarray/Substring Property Calculation**:
   - Example: Dynamically calculating sums, products, or maximum/minimum values
4. **Pattern Matching**:
   - Example: Detecting valid substrings that meet complex pattern requirements

### Edge Cases

When utilizing the sliding window technique, it's essential to consider edge cases to avoid errors in extreme scenarios. Key edge cases for this problem include:

1. **All Ones**:
   - Input: [1, 1, 1]
   - Output: Must subtract one, resulting in length len(nums) - 1
2. **All Zeros**:
   - Input: [0, 0, 0]
   - Output: No valid subarrays exist, resulting in output 0
3. **Empty Input**:
   - Input: []
   - Output: Should return 0 as there are no valid subarrays
4. **Single Element**:
   - Input: [1] or [0]
   - Output: Results in 0 due to the removal leaving no elements

### Summary

By mastering the dynamic sliding window approach, you can efficiently solve a wide variety of problems that require handling variable-sized subarrays or substrings. Its flexibility and high efficiency make it a crucial technique for algorithmic problem-solving.

---

## Part 3: Exact Count Dynamic Sliding Window

### Core Concept

The Exact Count Sliding Window is a specialized variant of the dynamic sliding window technique, designed to solve subarray problems that require "exactly meeting" certain conditions. This type of problem typically employs a "dual-pointer control group" approach, using two sliding windows to calculate differences.

### Key Features

1. **Dual Window Technique**:
   - Window 1: Tracks counts greater than or equal to the target value
   - Window 2: Tracks counts greater than or equal to the target value + 1
   - Final Result: Count from Window 1 - Count from Window 2

2. **Application Scenarios**:
   - Finding the number of subarrays with sum exactly equal to K
   - Finding subarrays containing exactly K specific elements
   - Scenarios requiring exact counting rather than finding max/min values

### Universal Template

Below is a universal template for solving exact count sliding window problems:

```java
/**
 * Universal template for exact count sliding window
 * Suitable for problems requiring counting subarrays that exactly meet certain conditions
 * @param nums input array
 * @param target target value
 * @return count of subarrays meeting the condition
 */
public static int countExactWindow(int[] nums, int target) {
    int result = 0;
    int value1 = 0;  // value for first window (>= target)
    int value2 = 0;  // value for second window (>= target + 1)

    // l1: left boundary of first window
    // l2: left boundary of second window
    // r: shared right boundary for both windows
    for (int l1 = 0, l2 = 0, r = 0; r < nums.length; r++) {
        // 1. Expand windows
        value1 += nums[r];
        value2 += nums[r];

        // 2. Contract first window
        while (l1 <= r && value1 >= target) {
            value1 -= nums[l1];
            l1++;
        }

        // 3. Contract second window
        while (l2 <= r && value2 >= target + 1) {
            value2 -= nums[l2];
            l2++;
        }

        // 4. Calculate result at current position: difference between windows
        result += l1 - l2;
    }
    
    return result;
}
```

### Technical Points

1. **Dual Window Maintenance**:
   - Maintain two windows, each with its own left pointer
   - Share the right pointer to ensure synchronized expansion
   - Window values must be updated synchronously

2. **Window Contraction Conditions**:
   - First Window: Contract when value >= target
   - Second Window: Contract when value >= target + 1
   - Update corresponding window values during contraction

3. **Result Calculation**:
   - Use l1 - l2 to calculate valid subarray count at each position
   - Accumulate counts at each position for final result

### Implementation Details

1. **Variable Design**:
   - result: stores the final count
   - value1, value2: store current values for both windows
   - l1, l2: represent left boundaries of both windows
   - r: shared right boundary

2. **Loop Structure**:
   - Outer Loop: traverse array, expand windows
   - Inner Loops: contract windows based on conditions

3. **Boundary Handling**:
   - Ensure l1, l2 don't exceed r
   - Carefully update values during window contraction

### Complexity Analysis

- **Time Complexity**: O(n)
   - Each element is added and removed at most once
   - Operations for both windows are linear

- **Space Complexity**: O(1)
   - Uses only constant extra space
   - No additional data structures required

### Use Cases

1. **Counting Problems**:
   - Subarrays with specific sum
   - Subarrays containing specific number of elements
   - Subsequences meeting exact conditions

2. **Pattern Matching**:
   - Subarrays with fixed number of features
   - Exact pattern matching requirements

3. **Statistical Analysis**:
   - Interval sum statistics
   - Frequency counting
   - Exact property statistics

### Key Considerations

1. **Initialization**:
   - Ensure correct initialization of window values
   - Set proper initial positions for boundary pointers

2. **Update Logic**:
   - Maintain synchronized window expansion
   - Properly update window values

3. **Result Accumulation**:
   - Understand the meaning of l1 - l2
   - Update result at correct timing

### Summary

The Exact Count Sliding Window is an elegant solution particularly suited for subarray problems requiring precise counting. By maintaining the difference between two windows, we can efficiently find all subarrays meeting exact conditions. The key aspects are:

1. Understanding the relationship between dual windows
2. Correctly maintaining window expansion and contraction
3. Accurately calculating result accumulation
4. Handling boundary conditions

Mastering this technique enables solving various subarray problems requiring exact counting, especially when finding subarrays that "exactly meet" certain conditions.