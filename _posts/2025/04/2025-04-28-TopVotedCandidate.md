---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "911. Online Election"
date: "2025-04-28"
tags: Medium
categories:
  - "LeetCode Binary Search"
---


- You are given two integer arrays `persons` and `times`. In an election, the `ith` vote was cast for `persons[i]` at time `times[i]`.
- For each query at a time `t`, find the person that was leading the election at time `t`. Votes cast at time `t` will count towards our query. In the case of a tie, the most recent vote (among tied candidates) wins.
- Implement the `TopVotedCandidate` class:
  - `TopVotedCandidate(int[] persons, int[] times)` Initializes the object with the `persons` and `times` arrays.
  - `int q(int t)` Returns the number of the person that was leading the election at time `t` according to the mentioned rules.


**Example 1**

```
Input
["TopVotedCandidate", "q", "q", "q", "q", "q", "q"]
[[[0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]], [3], [12], [25], [15], [24], [8]]
Output
[null, 0, 1, 1, 0, 0, 1]

Explanation
TopVotedCandidate topVotedCandidate = new TopVotedCandidate([0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]);
topVotedCandidate.q(3); // return 0, At time 3, the votes are [0], and 0 is leading.
topVotedCandidate.q(12); // return 1, At time 12, the votes are [0,1,1], and 1 is leading.
topVotedCandidate.q(25); // return 1, At time 25, the votes are [0,1,1,0,0,1], and 1 is leading (as ties go to the most recent vote.)
topVotedCandidate.q(15); // return 0
topVotedCandidate.q(24); // return 0
topVotedCandidate.q(8); // return 1
```

## Method 1

```tex
【O(N) build, O(logN) query time | O(N) space】
```

```java
package Leetcode.BinarySearch;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/04/28
 */
public class TopVotedCandidate {
    // Stores the leading candidate at each time point
    List<Integer> tops;
    // Maps each candidate to their current vote count
    Map<Integer, Integer> voteCounts;
    // Array of timestamps when votes were cast
    int[] times;

    public TopVotedCandidate(int[] persons, int[] times) {
        tops = new ArrayList<Integer>();
        voteCounts = new HashMap<Integer, Integer>();
        // Initialize with -1 as baseline for comparison
        voteCounts.put(-1, -1);
        int top = -1;  // Current leading candidate

        // Process each vote
        for (int i = 0; i < persons.length; ++i) {
            int p = persons[i];
            // Update vote count for current candidate
            voteCounts.put(p, voteCounts.getOrDefault(p, 0) + 1);
            // Update leader if current candidate has equal or more votes
            // This handles tie-breaking by favoring the most recent vote
            if (voteCounts.get(p) >= voteCounts.get(top)) {
                top = p;
            }
            tops.add(top);
        }
        this.times = times;
    }

    public int q(int t) {
        int l = 0, r = times.length - 1;
        // Binary search to find largest time point <= t
        while (l < r) {
            int m = l + (r - l + 1) / 2;
            if (times[m] <= t) {
                l = m;
            } else {
                r = m - 1;
            }
        }
        return tops.get(l);
    }
    
    public static void main(String[] args) {
        // Test case initialization
        int[] persons = {0, 1, 1, 0, 0, 1, 0};
        int[] times = {0, 5, 10, 15, 20, 25, 30};

        TopVotedCandidate tvc = new TopVotedCandidate(persons, times);

        // Test cases
        System.out.println(tvc.q(3));   // Expected output: 0
        System.out.println(tvc.q(12));  // Expected output: 1
        System.out.println(tvc.q(25));  // Expected output: 1
        System.out.println(tvc.q(15));  // Expected output: 0
        System.out.println(tvc.q(24));  // Expected output: 0
        System.out.println(tvc.q(8));   // Expected output: 1
    }
}
```





