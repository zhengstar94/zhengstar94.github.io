---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1462. Course Schedule IV"
date: "2025-01-27"
tags: Medium
categories:
  - "LeetCode Dynamic Programming"
---


- There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `ai` first if you want to take course `bi`.
  - For example, the pair `[0, 1]` indicates that you have to take course `0` before you can take course `1`.
- Prerequisites can also be **indirect**. If course `a` is a prerequisite of course `b`, and course `b` is a prerequisite of course `c`, then course `a` is a prerequisite of course `c`.
- You are also given an array `queries` where `queries[j] = [uj, vj]`. For the `jth` query, you should answer whether course `uj` is a prerequisite of course `vj` or not.
- Return *a boolean array* `answer`*, where* `answer[j]` *is the answer to the* `jth` *query.*

**Example 1**

```
Input: numCourses = 2, prerequisites = [ [ 1,0 ] ], queries = [ [ 0,1],[1,0 ] ]
Output: [false,true]
Explanation: The pair [1, 0] indicates that you have to take course 1 before you can take course 0.
Course 0 is not a prerequisite of course 1, but the opposite is true.
```

**Example 2**

```
Input: numCourses = 2, prerequisites = [], queries = [ [ 1,0],[0,1 ] ]
Output: [false,false]
Explanation: There are no prerequisites, and each course is independent.
```

**Example 3**

```
Input: numCourses = 3, prerequisites = [ [ 1,2],[1,0],[2,0 ] ], queries = [ [ 1,0],[1,2 ] ]
Output: [true,true]
```

## Method 1

```tex
【O(n³) time | O(n²) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/27
 */
public class CourseScheduleIV {

    public static List<Boolean> checkIfPrerequisite(int numCourses, int[][] prerequisites, int[][] queries) {
        // Create adjacency matrix to represent course dependencies
        // connected[i][j] = true means course i is a prerequisite of course j
        boolean[][] connected = new boolean[numCourses][numCourses];

        // Initialize direct prerequisites relationships
        // For each pair [a,b], mark that course 'a' is a direct prerequisite of course 'b'
        for (int[] prereq : prerequisites) {
            connected[prereq[0]][prereq[1]] = true;
        }

        // Floyd-Warshall Algorithm Implementation
        // This algorithm finds all possible paths between any two courses (direct and indirect prerequisites)
        for (int k = 0; k < numCourses; k++) {           // k represents the intermediate course being considered
            for (int i = 0; i < numCourses; i++) {       // i represents the starting course
                for (int j = 0; j < numCourses; j++) {   // j represents the target course
                    // For each trio of courses (i,j,k), check if:
                    // 1. Either there's already a path from i to j (connected[i][j])
                    // 2. OR there's a path from i to k AND from k to j
                    // If either condition is true, then course i is a prerequisite of course j
                    connected[i][j] = connected[i][j] ||
                            (connected[i][k] && connected[k][j]);
                }
            }
        }

        // Process each query to build the result list
        List<Boolean> result = new ArrayList<>();
        for (int[] query : queries) {
            // For each query [u,v], check if course u is a prerequisite of course v
            // by looking up the value in our processed adjacency matrix
            result.add(connected[query[0]][query[1]]);
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1
        // Course structure: 1 -> 0 (Course 1 is a prerequisite of Course 0)
        int numCourses1 = 2;
        int[][] prerequisites1 = { {1,0}};
        int[][] queries1 = { { 0,1},{1,0 } };
        System.out.println("Test Case 1 Result: " +
                checkIfPrerequisite(numCourses1, prerequisites1, queries1));
        // Expected Output: [false,true]

        // Test Case 2
        // Course structure: 1 -> 2 -> 0 and 1 -> 0 (direct)
        int numCourses2 = 3;
        int[][] prerequisites2 = { { 1,2},{1,0},{2,0 } };
        int[][] queries2 = { { 1,0},{1,2 } };
        System.out.println("Test Case 2 Result: " +
                checkIfPrerequisite(numCourses2, prerequisites2, queries2));
        // Expected Output: [true,true]
    }
}

```





