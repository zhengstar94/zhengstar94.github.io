---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "40. Combination Sum II"
date: "2025-01-26"
tags: Medium
categories:
  - "LeetCode Backtracking"
---


- Given a collection of candidate (n. 申请求职者, 候选人 报考者) numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate (n. 申请求职者, 候选人 报考者) numbers sum to `target`.
- Each number in `candidates` may only be used **once** in the combination.
- **Note:** The solution set must not contain duplicate combinations.

**Example 1**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2**

```
Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
```

## Method 1

```tex
【O(2^n) time | O(n) space】
```

```java
package Leetcode.Backtracking;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/26
 */
public class CombinationSumII {

    public static List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> results = new ArrayList<>();
        // Sort array first to handle duplicates and enable pruning
        Arrays.sort(candidates);
        backtrack(results, new ArrayList<>(), candidates, target, 0);
        return results;
    }

    private static void backtrack(List<List<Integer>> results, List<Integer> temp,
                                  int[] candidates, int remain, int start) {
        // Base case: if remaining sum is 0, we found a valid combination
        if (remain == 0) {
            // Add a deep copy of current combination to results
            results.add(new ArrayList<>(temp));
            return;
        }

        // Try each candidate number starting from 'start' index
        for (int i = start; i < candidates.length; i++) {
            // Pruning: if current number is greater than remaining sum,
            // all subsequent numbers will also be too large (array is sorted)
            if (candidates[i] > remain) {
                break;
            }

            // Skip duplicates in the same level of recursion tree
            // This prevents generating duplicate combinations
            // Only skip if it's not the first element in current recursion level
            if (i > start && candidates[i] == candidates[i-1]) {
                continue;
            }

            // Include current number in combination
            temp.add(candidates[i]);

            // Recursive call:
            // - Subtract current number from remaining sum
            // - Start from next index (i+1) as each number can only be used once
            backtrack(results, temp, candidates, remain - candidates[i], i + 1);

            // Backtrack: remove current number to try next possibility
            temp.remove(temp.size() - 1);
        }
    }
    
    public static void main(String[] args) {
        // Test case 1: Expected output: [[1,1,6], [1,2,5], [1,7], [2,6]]
        int[] candidates1 = {10,1,2,7,6,1,5};
        int target1 = 8;
        System.out.println("Test case 1 result:");
        System.out.println(combinationSum2(candidates1, target1));

        // Test case 2: Expected output: [[1,2,2], [5]]
        int[] candidates2 = {2,5,2,1,2};
        int target2 = 5;
        System.out.println("Test case 2 result:");
        System.out.println(combinationSum2(candidates2, target2));
    }
}
```





