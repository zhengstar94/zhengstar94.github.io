---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Task Assignment"
date: "2023-04-11"
categories:
  - "Algorithm Greedy"
---

# Task Assignment [Medium]

- You're given an integer k representing a number of workers and an array of positive integers representing durations of tasks that must be completed by the workers. Specifically, each worker must complete two unique tasks and can only work on one task at a time. The number of tasks will always be equal to 2k such that each worker always has exactly two tasks to complete. All tasks are independent of one another and can be completed in any order. Workers will complete their assigned tasks in parallel, and the time taken to complete all tasks will be equal to the time taken to complete the longest pair of tasks (see the sample output for an explanation).
- Write a function that returns the optimal assignment of tasks to each worker such that the tasks are completed as fast as possible. Your function should return a list of pairs, where each pair stores the indices of the tasks that should be completed by one worker. The pairs should be in the following format: [task1, task2], where the order of task1 and task2 doesn't matter. Your function can return the pairs in any order. If multiple optimal assignments exist, any correct answer will be accepted.
- Note: you'll always be given at least one worker (i.e., k will always be greater than 0).

**Sample Input**

```
k = 3
tasks = [1, 3, 5, 3, 1, 4]
```

**Sample Output**

```
[
  [0, 2], // tasks[0] = 1, tasks[2] = 5 | 1 + 5 = 6
  [4, 5], // tasks[4] = 1, tasks[5] = 4 | 1 + 4 = 5
  [1, 3], // tasks[1] = 3, tasks[3] = 3 | 3 + 3 = 6
] // The fastest time to complete all tasks is 6.

// Note: there are multiple correct answers for this sample input.
// The following is an example of another correct answer:
// [
//   [2, 4],
//   [0, 5],
//   [1, 3]
// ]
```

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Start by considering which pairs of tasks will lead to the longest possible time to complete all tasks.</strong></i>
</details>


<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The amount of time it'll take to complete all tasks will be dictated by the pair of tasks that has the longest total duration. This means that you'll want to avoid pairing long tasks together. </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Since the pair of tasks with the longest total duration is the time it takes for us to finish all tasks, we want to minimize this pair's duration. To do this, we can simply pair the shortest-duration task with the longest-duration task and repeat the process with all other tasks.</strong></i>
</details>


<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Start by sorting the tasks array in ascending order. Then, pair the shortest-duration task with the longest-duration task, and add that pair to some output array. Repeat this process until you've paired all tasks. This will lead to an optimal pairing, because your pair of tasks with the longest duration will have the shortest duration that it can possibly have. </strong></i>
</details>

<br>

## Method 1

```tex
【O(nlog(n))time∣O(n)space】
```

```java
package Greedy;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/05/11
 */
public class TaskAssignment {
    /**
     * Task Assignment
     *
     * @param k     the number of teams
     * @param tasks the task list
     * @return the list of team indices corresponding to the tasks
     */
    public static ArrayList<ArrayList<Integer>> taskAssignment(int k, ArrayList<Integer> tasks) {
        // initialize the result list
        ArrayList<ArrayList<Integer>> result = new ArrayList<>();
        // count the positions of each task
        HashMap<Integer, ArrayList<Integer>> indices = getIndices(tasks);

        // sort the tasks
        Collections.sort(tasks);

        int start = 0;
        int end = 2 * k - 1;

        // pair up the tasks
        while (start < end) {
            // get the smallest and largest tasks in the current task list
            int firstTask = tasks.get(start);
            int secondTask = tasks.get(end);

            // get the index lists of the smallest and largest tasks
            ArrayList<Integer> firstTaskIndices = indices.get(firstTask);
            ArrayList<Integer> secondTaskIndices = indices.get(secondTask);

            // get the last index of the smallest and largest tasks
            int firstTaskIndex = firstTaskIndices.get(firstTaskIndices.size() - 1);
            int secondTaskIndex = secondTaskIndices.get(secondTaskIndices.size() - 1);

            // remove the matched indices
            firstTaskIndices.remove(firstTaskIndices.size() - 1);
            secondTaskIndices.remove(secondTaskIndices.size() - 1);

            // add the matched index pairs
            ArrayList<Integer> pair = new ArrayList<>();
            pair.add(firstTaskIndex);
            pair.add(secondTaskIndex);

            result.add(pair);
            start++;
            end--;
        }

        // return the matched result list
        return result;
    }

    /**
     * Count the positions of each task
     *
     * @param tasks the task list
     * @return the hash table of task positions
     */
    public static HashMap<Integer, ArrayList<Integer>> getIndices(ArrayList<Integer> tasks) {
        // initialize the hash table
        HashMap<Integer, ArrayList<Integer>> indices = new HashMap<>();

        // traverse the task list
        for (int i = 0; i < tasks.size(); i++) {
            int task = tasks.get(i);
            // if the task already exists in the hash table, add the current position to its index list
            if (indices.containsKey(task)) {
                ArrayList<Integer> arrOfIndices = indices.get(task);
                arrOfIndices.add(i);
            }
            // if the task does not exist in the hash table, create a new index list and add the current position to it
            else {
                ArrayList<Integer> arrOfIndices = new ArrayList<>();
                arrOfIndices.add(i);
                indices.put(task, arrOfIndices);
            }
        }

        // return the hash table of task positions
        return indices;
    }

    public static void main(String[] args) {
        Integer[] intArr = new Integer[]{1, 3, 5, 3, 1, 4};
        ArrayList<Integer> task = new ArrayList<>(Arrays.asList(intArr));

        System.out.println(taskAssignment(3,task));
    }
}


```

