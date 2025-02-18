---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "16. 3Sum Closest"
date: "2024-11-13"
categories:
  - "LeetCode TwoPointers"
---

- Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.
- Return *the sum of the three integers*.
- You may assume that each input would have exactly one solution.

**Example 1**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Example 2**

```
Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
```

## Method 1

```tex
【O(n^2) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/11/13
 */
public class ThreeSumClosest {

    public static int threeSumClosest(int[] nums, int target) {
        // Sort the array first to use two-pointer technique
        // This allows us to move pointers based on sum comparison
        Arrays.sort(nums);

        // Initialize result with first three numbers
        // This ensures we have a valid sum to start with
        int result = nums[0] + nums[1] + nums[2];

        // Iterate through the array, fixing the first number
        // We stop at length-2 because we need at least 2 more numbers after i
        for (int i = 0; i < nums.length - 2; i++){
            // Skip duplicates for the first number to avoid duplicate combinations
            // This optimization reduces unnecessary calculations
            if(i > 0 && nums[i] == nums[i - 1]){
                continue;
            }

            // Initialize two pointers:
            // left: starts right after current number i
            // right: starts from the end of array
            int left = i + 1;
            int right = nums.length - 1;

            // Use two pointers to find the other two numbers
            while (left < right){
                // Calculate current sum of three numbers
                int currentSum = nums[i] + nums[left] + nums[right];

                // If we find an exact match, we can return immediately
                // as this is the closest possible sum to target
                if(currentSum == target){
                    return currentSum;
                }

                // Update result if current sum is closer to target than previous result
                // Using absolute difference to compare distances to target
                if(Math.abs(currentSum - target) < Math.abs(result - target)){
                    result = currentSum;
                }

                // If current sum is less than target, we need a larger sum
                // So move left pointer to the right to get larger numbers
                if(currentSum < target){
                    left++;

                    // Skip duplicates for the second number
                    // This optimization avoids duplicate combinations
                    while (left < right && nums[left] == nums[left - 1]){
                        left++;
                    }
                }
                // If current sum is greater than target, we need a smaller sum
                // So move right pointer to the left to get smaller numbers
                else{
                    right--;

                    // Skip duplicates for the third number
                    // This optimization avoids duplicate combinations
                    while (left < right && nums[right] == nums[right + 1]){
                        right--;
                    }
                }
            }
        }
        // Return the closest sum found
        return result;
    }


    public static void main(String[] args) {
        // Test case
        // nums = [ -1, 2, 1, -4 ], target = 1
        // Expected output: 2 (sum of -1 + 1 + 2)
        int[] nums = {-1, 2, 1, -4};
        int target = 1;

        // Print input array, target value, and result
        System.out.println("Input array: " + Arrays.toString(nums));
        System.out.println("Target value: " + target);
        System.out.println("Closest three sum: " + threeSumClosest(nums, target));
    }
}
```





