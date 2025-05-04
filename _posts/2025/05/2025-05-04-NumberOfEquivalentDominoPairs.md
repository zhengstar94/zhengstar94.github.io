---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1128. Number of Equivalent Domino Pairs"
date: "2025-05-04"
tags: Easy
categories:
  - "LeetCode HashTable"
---

- Given a list of `dominoes`, `dominoes[i] = [a, b]` is **equivalent to** `dominoes[j] = [c, d]` if and only if either (`a == c` and `b == d`), or (`a == d` and `b == c`) - that is, one domino can be rotated to be equal to another domino.
- Return *the number of pairs* `(i, j)` *for which* `0 <= i < j < dominoes.length`*, and* `dominoes[i]` *is **equivalent to*** `dominoes[j]`.

**Example 1**

```
Input: dominoes = [ [ 1,2],[2,1],[3,4],[5,6 ] ]
Output: 1
```

**Example 2**

```
Input: dominoes = [ [ 1,2],[1,2],[1,1],[1,2],[2,2 ] ]
Output: 3
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/05/04
 */
public class NumberOfEquivalentDominoPairs {

    public static int numEquivDominoPairs(int[][] dominoes) {
        // HashMap to store the count of each unique domino type
        Map<Integer, Integer> countMap = new HashMap<>();
        // Counter for the total number of equivalent pairs
        int pairs = 0;

        // Iterate through each domino in the array
        for (int[] domino : dominoes) {
            // Create a unique hash value for each domino, ensuring equivalent dominoes have the same hash
            // We use min*10 + max to ensure [a,b] and [b,a] get the same hash value
            // This works because in this problem, domino values are between 1-9
            int key = Math.min(domino[0], domino[1]) * 10 + Math.max(domino[0], domino[1]);

            // Get the current count of dominoes with the same hash value (same type)
            // If this is the first time we see this type, getOrDefault returns 0
            int count = countMap.getOrDefault(key, 0);

            // The current domino can form a pair with each previously seen domino of the same type
            // So we add the current count to our running total of pairs
            pairs += count;

            // Update the count of this domino type in our map
            // We increment by 1 to account for the current domino
            countMap.put(key, count + 1);
        }

        // Return the total number of equivalent domino pairs found
        return pairs;
    }
    
    public static void main(String[] args) {
        // Test case 1: Should return 1 pair
        // [1,2] and [2,1] are equivalent
        int[][] dominoes1 = { { 1, 2}, {2, 1}, {3, 4}, {5, 6 } };
        System.out.println("Test case 1 result: " + numEquivDominoPairs(dominoes1)); // Should output 1

        // Test case 2: Should return 3 pairs
        // The pairs are: ( [ 1,2], [1,2]), ([1,2], [1,2]), ([1,2], [1,2 ] )
        // Note: These are the pairs at indices (0,1), (0,3), and (1,3)
        int[][] dominoes2 = { { 1, 2}, {1, 2}, {1, 1}, {1, 2}, {2, 2 } };
        System.out.println("Test case 2 result: " + numEquivDominoPairs(dominoes2)); // Should output 3

        // Additional test case: Should return 4 pairs
        // The pairs are:
        // ( [ 1,1], [1,1]), ([1,1], [1,1]), ([1,1], [1,1]), ([1,2], [1,2 ] )
        // These are the pairs at indices (0,2), (0,5), (2,5), and (3,4)
        int[][] dominoes3 = { { 1, 1}, {2, 2}, {1, 1}, {1, 2}, {1, 2}, {1, 1 } };
        System.out.println("Test case 3 result: " + numEquivDominoPairs(dominoes3)); // Should output 4
    }
}
```





