---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "377.Combination Sum IV"
date: "2024-10-20"
categories:
  - "LeetCode Dynamic Programming"
---


- Given an array of **distinct** integers `nums` and a target integer `target`, return *the number of possible combinations that add up to* `target`.
- The test cases are generated so that the answer can fit in a **32-bit** integer.


**Example 1**

```
Input: nums = [ 1,2,3 ], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```

**Example 2**

```
Input: nums = [9], target = 3
Output: 0
```

## Method 1

```tex
【O(n * target) time | O(target) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/10/20
 */
public class CombinationSumIV {
    public static int combinationSum4(int[] nums, int target) {
        // Create a DP array where dp[i] will store the number of combinations to reach the sum i.
        int[] dp = new int[target + 1];

        // Base case: There is one way to make the sum 0, which is to choose nothing.
        dp[0] = 1;

        // Iterate through each possible sum from 1 to the target.
        for (int i = 1; i <= target; i++) {
            // For each number in the provided nums array.
            for (int num : nums) {
                // If the current sum i is greater than or equal to num,
                // it means we can use this num to help form the sum i.
                if (i >= num) {
                    // Add the number of combinations to form the sum (i - num)
                    // to the current sum i. This is because each combination
                    // that makes up (i - num) can be extended to form the sum i
                    // by adding the current num.
                    dp[i] += dp[i - num];
                }
            }
        }

        // Return the total combinations to reach the target sum.
        return dp[target];
    }

    public static void main(String[] args) {
        // Test case 1: nums = [ 1, 2, 3 ], target = 4
        // Expected output: 7
        int[] nums1 = { 1, 2, 3 };
        int target1 = 4;
        System.out.println("Test Case 1: " + combinationSum4(nums1, target1)); // Output: 7

        // Test case 2: nums = [ 2, 1 ], target = 5
        // Expected output: 8
        int[] nums2 = { 2, 1 };
        int target2 = 5;
        System.out.println("Test Case 2: " + combinationSum4(nums2, target2)); // Output: 8

        // Test case 3: nums = [ 1 ], target = 0
        // Expected output: 1 (there is one way to sum to zero: choose nothing)
        int[] nums3 = { 1 };
        int target3 = 0;
        System.out.println("Test Case 3: " + combinationSum4(nums3, target3)); // Output: 1

        // Test case 4: nums = [ 3, 5, 8 ], target = 10
        // Expected output: 1 (the only way is to choose 2 threes and one one, or two fives)
        int[] nums4 = { 3, 5, 8 };
        int target4 = 10;
        System.out.println("Test Case 4: " + combinationSum4(nums4, target4)); // Output: 1

        // Test case 5: nums = [ 1, 4, 5 ], target = 6
        // Expected output: 6
        int[] nums5 = { 1, 4, 5 };
        int target5 = 6;
        System.out.println("Test Case 5: " + combinationSum4(nums5, target5)); // Output: 6
    }
}

```





