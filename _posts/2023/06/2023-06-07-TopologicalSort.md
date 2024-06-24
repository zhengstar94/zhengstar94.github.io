---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Topological Sort"
date: "2023-06-07"
categories:
  - "Algorithm Famous"
---

# Topological Sort [Hard]

- You're given a list of arbitrary jobs that need to be completed; these jobs are represented by distinct integers. You're also given a list of dependencies. A dependency is represented as a pair of jobs where the first job is a prerequisite of the second one. In other words, the second job depends on the first one; it can only be completed once the first job is completed.
- Write a function that takes in a list of jobs and a list of dependencies and returns a list containing a valid order in which the given jobs can be completed. If no such order exists, the function should return an empty array.

**Sample Input**

```
jobs = [1, 2, 3, 4]
deps = [[1, 2], [1, 3], [3, 2], [4, 2], [4, 3]]
```

**Sample Output**

```
[1, 4, 3, 2] or [4, 1, 3, 2]
```

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try representing the jobs and dependencies as a graph, where each vertex is a job and each edge is a dependency. How can you traverse this graph to topologically sort the list of jobs? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> One approach to solving this problem is to traverse the graph mentioned in Hint #1 using Depth-first Search. Starting at a random job, traverse its prerequisite jobs in Depth-first Search fashion until you reach a job with no prerequisites; such a job can safely be appended to the final order. Once you've traversed and added all prerequisites of a job to the final order, you can append the job in question to the order. This approach will have to track whether nodes have been traversed already, whether they're in the process of being traversed (which would indicate a cycle in the graph and therefore no valid topological order), or whether they're ready to be traversed. </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Another approach to solving this problem is to traverse the graph mentioned in Hint #1 starting specifically with jobs that have no prerequisites. Keep track of all the jobs that have no prerequisites, traverse them one by one, and append them to the final order. For all of these jobs, remove their dependencies from the graph and update the number of prerequisites for each of these dependencies accordingly (these dependencies should now have one prerequisite less since one of their prerequisite job has just been added to the final order). As you update the number of prerequisites for these other jobs, keep track of the ones that no longer have prerequisites and that are ready to be traversed. You'll eventually go through all of the jobs if there are no cycles in the graph. If there is a cycle in the graph, there will still be jobs with prerequisites and you'll know that there is no valid topological order. This approach will involve keeping track of the number of prerequisites per job as well as all the actual dependencies of each job. </strong></i>
</details>

<br>

## Method 1

```tex
【O(j + d)time∣O(j + d)space】
```

```tex
1.\ Where\ j\ is\ the\ number\ of\ jobs\ and\ d\ is\ the\ number\ of\ dependencies\\
```

```java
package Famous;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/11/25
 */
public class TopologicalSort {
    
    public static List<Integer> topologicalSort(List<Integer> jobs, List<int[]> deps) {
        // Adjacency list to store the graph
        Map<Integer, List<Integer>> graph = new HashMap<>();
        // Map to store in-degrees for each node
        Map<Integer, Integer> inDegrees = new HashMap<>();

        // Initialize the graph and in-degrees
        for (int job : jobs) {
            graph.put(job, new ArrayList<>());
            inDegrees.put(job, 0);
        }

        // Build the graph and calculate in-degrees
        for (int[] dep : deps) {
            int prereq = dep[0];
            int job = dep[1];
            graph.get(prereq).add(job);
            inDegrees.put(job, inDegrees.get(job) + 1);
        }

        // List to store the final sorted order
        List<Integer> result = new ArrayList<>();
        // Queue for BFS
        Queue<Integer> queue = new LinkedList<>();

        // Add nodes with in-degree 0 to the queue
        for (int job : jobs) {
            if (inDegrees.get(job) == 0) {
                queue.offer(job);
            }
        }

        // Perform topological sort using BFS
        while (!queue.isEmpty()) {
            int currentJob = queue.poll();
            result.add(currentJob);

            // Update in-degrees of dependent nodes
            for (int dependentJob : graph.get(currentJob)) {
                inDegrees.put(dependentJob, inDegrees.get(dependentJob) - 1);
                // If in-degree becomes 0, add the node to the queue
                if (inDegrees.get(dependentJob) == 0) {
                    queue.offer(dependentJob);
                }
            }
        }

        // Check if a valid topological sort exists
        if (result.size() != jobs.size()) {
            return new ArrayList<>();
        }

        return result;
    }

    public static void main(String[] args) {
        // Test data
        List<Integer> jobs = Arrays.asList(1, 2, 3, 4);
        List<int[]> deps = Arrays.asList(new int[]{1, 2}, new int[]{1, 3}, new int[]{3, 2}, new int[]{4, 2}, new int[]{4, 3});

        // Perform topological sort and print the result
        List<Integer> result = topologicalSort(jobs, deps);
        System.out.println(result);
    }
}

```









