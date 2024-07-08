---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "39.Combination Sum"
date: "2024-07-08"
categories:
  - "LeetCode Backtracking"
---

# 39. Combination Sum

- Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.
- The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.
- The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3**

```
Input: candidates = [2], target = 1
Output: []
```

## Method 1

```tex
【O(n^t) time | O(t) space】
```

```java
package Leetcode.Backtracking;

import java.util.ArrayList;
import java.util.List;

/**
 * This class implements the combination sum problem using backtracking algorithm.
 *
 * For given distinct integers and a target sum, it finds all unique combinations
 * where the chosen numbers sum to the target. The same number can be chosen
 * from candidates an unlimited number of times.
 *
 * @author zhengstars
 * @date 2024/07/08
 */
public class CombinationSum {

    /**
     * Finds all unique combinations that sum up to the target.
     *
     * @param candidates an array of distinct integers
     * @param target the target sum
     * @return a list of all unique combinations that sum up to the target
     */
    public static List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(candidates, target, 0, new ArrayList<>(), result);
        return result;
    }

    /**
     * The backtracking function to search for combinations.
     *
     * @param candidates an array of distinct integers
     * @param target the remaining target sum
     * @param start the starting index of the search
     * @param current the current combination being constructed
     * @param result the list to store valid combinations
     */
    private static void backtrack(int[] candidates, int target, int start, List<Integer> current, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(current));
            return;
        }

        if (target < 0) {
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            current.add(candidates[i]);

            // Recursive call to search for combinations
            backtrack(candidates, target - candidates[i], i, current, result);

            // Backtrack by removing the last element
            current.remove(current.size() - 1);
        }
    }

    /**
     * Main method to test the combinationSum function.
     */
    public static void main(String[] args) {
        int[] candidates1 = {2, 3, 6, 7};
        int target1 = 7;
        System.out.println(combinationSum(candidates1, target1)); // [ [2, 2, 3], [7]]

        int[] candidates2 = {2, 3, 5};
        int target2 = 8;
        System.out.println(combinationSum(candidates2, target2)); // [ [2, 2, 2, 2], [2, 3, 3], [3, 5]]

        int[] candidates3 = {2};
        int target3 = 1;
        System.out.println(combinationSum(candidates3, target3)); // []
    }
}
```

