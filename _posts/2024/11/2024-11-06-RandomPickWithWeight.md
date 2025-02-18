---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "528.Random Pick with Weight"
date: "2024-11-06"
categories:
  - "LeetCode TwoPointers"
---

- You are given a **0-indexed** array of positive integers `w` where `w[i]` describes the **weight** of the `ith` index.
- You need to implement the function `pickIndex()`, which **randomly** picks an index in the range `[0, w.length - 1]` (**inclusive**) and returns it. The **probability** of picking an index `i` is `w[i] / sum(w)`.
  - For example, if `w = [1, 3]`, the probability of picking index `0` is `1 / (1 + 3) = 0.25` (i.e., `25%`), and the probability of picking index `1` is `3 / (1 + 3) = 0.75` (i.e., `75%`).

**Example 1**

```
Input
["Solution","pickIndex"]
[[[1]],[]]
Output
[null,0]

Explanation
Solution solution = new Solution([1]);
solution.pickIndex(); // return 0. The only option is to return 0 since there is only one element in w.
```

**Example 2**

```
Input
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
Output
[null,1,1,1,1,0]

Explanation
Solution solution = new Solution([1, 3]);
solution.pickIndex(); // return 1. It is returning the second element (index = 1) that has a probability of 3/4.
solution.pickIndex(); // return 1
solution.pickIndex(); // return 1
solution.pickIndex(); // return 1
solution.pickIndex(); // return 0. It is returning the first element (index = 0) that has a probability of 1/4.

Since this is a randomization problem, multiple answers are allowed.
All of the following outputs can be considered correct:
[null,1,1,1,1,0]
[null,1,1,1,1,1]
[null,1,1,1,0,0]
[null,1,1,1,0,1]
[null,1,0,1,0,0]
......
and so on.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Random;

/**
 * @author zhengstars
 * @date 2024/11/06
 */
public class RandomPickWithWeight {
    // Stores the prefix sum array for cumulative weights
    private int[] prefix;
    // Stores the total sum of all weights
    private int total;
    // Random number generator
    private Random rand;

    public RandomPickWithWeight(int[] w) {
        this.prefix = new int[w.length];
        this.rand = new Random();

        // Build prefix sum array
        // prefix[i] represents the sum of weights from index 0 to i
        prefix[0] = w[0];  // First element is just the first weight
        for (int i = 1; i < w.length; i++) {
            prefix[i] = prefix[i - 1] + w[i];  // Add current weight to previous sum
        }

        // Store total sum of weights for generating random numbers
        total = prefix[w.length - 1];
    }

    public int pickIndex() {
        // Generate random number in range [1, total]
        // Adding 1 ensures we never get 0, maintaining proper probability distribution
        int target = rand.nextInt(total) + 1;

        // Use binary search to find the index where target falls
        // This is equivalent to finding the smallest prefix sum >= target
        int left = 0, right = prefix.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;  // Calculate middle point safely
            if (prefix[mid] < target) {
                // If prefix sum at mid is less than target,
                // target must be in right half
                left = mid + 1;
            } else {
                // If prefix sum at mid is >= target,
                // this could be our answer or the answer is in left half
                right = mid;
            }
        }
        // The final value of left is our answer
        // It represents the index whose weight range contains our target
        return left;
    }
}

```





