---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "LCP 12. Zhang's Problem Solving Plan"
date: "2025-05-17"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.MinimizeMaximum"
---


- To improve his coding skills, Zhang has prepared a LeetCode problem-solving plan. He selected `n` problems from the LeetCode problem database, numbered from `0` to `n-1`, and plans to solve all problems **in order of their problem numbers** within `m` days (note that Zhang cannot spend multiple days on the same problem).
- In Zhang's plan, he needs `time[i]` units of time to complete problem number `i`. Additionally, Zhang can use an external help feature by asking his good friend Yang for the solution to a problem, which saves him the time needed for that problem. To prevent "Zhang's problem-solving plan" from becoming "Yang's problem-solving plan," Zhang can use this help feature at most once per day.
- We define `T` as the maximum time spent solving problems in a single day among the `m` days (problems completed by Yang do not count toward the total solving time). Please help Zhang find the minimum possible value of `T`.

**Example 1**

```
Input: time = [1,2,3,3], m = 2
Output: 3
Explanation: On the first day, Zhang completes the first three problems, with Yang's help for the third problem. On the second day, he completes the fourth problem and also asks Yang for help. This way, the maximum time spent on any day is 3, and this value is minimal.
```

**Example 2**

```
Input: time = [999,999,999], m = 4
Output: 0
Explanation: During the first three days, Zhang asks Yang for help once each day, allowing him to complete all problems within three days without spending any time himself.
```

## Method 1

```tex
【O(n * log(sum(time))) time | O(1) space】
```

```java
package Leetcode.BinarySearch.MinimizeMaximum;

/**
 * @author zhengxingxing
 * @date 2025/05/17
 */
public class ZhangsProblemSolvingPlan {

    public static int minTime(int[] time, int m) {
        // Edge case: if input is null, empty, or days <= 0, no time needed
        if (time == null || time.length == 0 || m <= 0) {
            return 0;
        }

        // If days >= number of problems, Zhang can do one problem per day,
        // use help for each to reduce time to zero
        if (m >= time.length) {
            return 0;
        }

        // Initialize binary search boundaries
        int left = 0;
        int right = 0;
        for (int t : time) {
            right += t; // maximum possible time is sum of all problem times (no help used)
        }

        // Binary search to find the minimal max time per day
        while (left < right) {
            int mid = left + (right - left) / 2;

            // Check if Zhang can finish all problems within m days
            // without exceeding mid as max daily solving time (after help)
            if (canFinish(time, m, mid)) {
                right = mid; // try smaller max time
            } else {
                left = mid + 1; // need to allow more time
            }
        }

        return left; // minimum max time found
    }
    
    private static boolean canFinish(int[] time, int m, int maxTime) {
        int days = 1;      // currently used days
        int sum = 0;       // sum of problem times for the current day
        int maxVal = 0;    // maximum problem time for the current day (help will be used here)

        for (int t : time) {
            sum += t;                // accumulate time for current day
            maxVal = Math.max(maxVal, t);  // track longest problem time in the day

            // Check if current day's time minus longest problem time exceeds maxTime
            // This simulates using help on the longest problem to reduce daily load
            if (sum - maxVal > maxTime) {
                days++;        // need an additional day
                sum = t;       // reset sum to current problem time for new day
                maxVal = t;    // reset maxVal for new day

                // If number of days exceeds allowed m, return false immediately
                if (days > m) {
                    return false;
                }
            }
        }

        // If all problems fit in m days under maxTime constraint, return true
        return true;
    }

    public static void main(String[] args) {
        // Test case 1: Example input 1
        int[] time1 = {1, 2, 3, 3};
        int m1 = 2;
        System.out.println("Test case 1, expected: 3, actual: " + minTime(time1, m1));

        // Test case 2: Example input 2, days more than problems
        int[] time2 = {999, 999, 999};
        int m2 = 4;
        System.out.println("Test case 2, expected: 0, actual: " + minTime(time2, m2));
    }
}

```





