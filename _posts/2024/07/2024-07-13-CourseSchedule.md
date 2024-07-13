---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "207.Course Schedule"
date: "2024-07-13"
categories:
  - "LeetCode Graphs"
---


# 207. Course Schedule

- There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.
  - For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.
- Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

## Method 1

```tex
【O(V + E) time | O(V + E) space】
```

```java
package Leetcode.Graphs;

import java.util.ArrayList;
import java.util.List;

/**
 *
 * @author zhengstars
 * @date 2024/07/13
 */
public class CourseSchedule {
    // Node visit status: 0-Not Visited, 1-Currently Visiting, 2-Visited
    private int[] visited;
    // Adjacency list representing the graph relationships
    private List<List<Integer>> adjacencyList;
    // Whether a cycle exists
    private boolean hasCycle;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        visited = new int[numCourses];
        adjacencyList = new ArrayList<>();

        // Initialize the adjacency list
        for (int i = 0; i < numCourses; i++) {
            adjacencyList.add(new ArrayList<>());
        }

        // Build the graph
        for (int[] pair : prerequisites) {
            adjacencyList.get(pair[1]).add(pair[0]);
        }

        // Check each node
        for (int i = 0; i < numCourses; i++) {
            if (visited[i] == 0) {
                dfs(i);
            }
        }

        return !hasCycle;
    }

    private void dfs(int course) {
        // Mark as currently visiting
        visited[course] = 1;

        // Recursively visit adjacent nodes
        for (int neighbor : adjacencyList.get(course)) {
            if (visited[neighbor] == 0) {
                dfs(neighbor);
                if (hasCycle) {
                    return;
                }
            } else if (visited[neighbor] == 1) {
                // Encountered a currently visiting node, indicating a cycle
                hasCycle = true;
                return;
            }
        }

        // Mark as visited
        visited[course] = 2;
    }

    public static void main(String[] args) {
        CourseSchedule cs = new CourseSchedule();

        int numCourses1 = 2;
        int[][] prerequisites1 = { {1, 0}};
        System.out.println(cs.canFinish(numCourses1, prerequisites1)); // Output: true

        int numCourses2 = 2;
        int[][] prerequisites2 = { {1, 0}, {0, 1}};
        //System.out.println(cs.canFinish(numCourses2, prerequisites2)); // Output: false
    }
}
```

