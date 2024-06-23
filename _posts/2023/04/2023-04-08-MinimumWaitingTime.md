---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Minimum Waiting Time"
date: "2023-04-08"
categories:
  - "Algorithm Greedy"
---

# Minimum Waiting Time [Easy]

- You're given a non-empty array of positive integers representing the amounts of time that specific queries take to execute. Only one query can be executed at a time, but the queries can be executed in any order.
- A query's **waiting time** is defined as the amount of time that it must wait before its execution starts. In other words, if a query is executed second, then its waiting time is the duration of the first query; if a query is executed third, then its waiting time is the sum of the durations of the first two queries.
- Write a function that returns the minimum amount of total waiting time for all of the queries. For example, if you're given the queries of durations `[1,4,5]`, then the total waiting time if the queries were executed in the order of `[5,1,4]`would be `(0)+(5)+(5 1)=11`. The first query of duration `5` would be executed immediately, so its waiting time would be `0`, the second query of duration `1` would have to wait `5` seconds (the duration of the first query) to be executed, and the last query would have to wait the duration of the first two queries before being executed.
- Note: you're allowed to mutate the input array.

**Sample Input**

```
queries=[3,2,1,2,6]
```

**Sample Output**

```
17
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Even though you don't need to return the actual order in which the queries will be executed to
minimize the total waiting time, it's important to determine what this order should be.Start by doing so. </strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Can you solve this problem with constant space? What advantage does being able to mutate the input array provide? </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Sort the input array in place,and execute the shortest queries in their sorted order. This should allow you to calculate the minimum waiting time.</strong></i>
</details>



<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Create a variable to store the total waiting time, and iterate through the sorted input array. At each iteration, multiply the number of queries left by the duration of the current query, and add that to the total waiting time.</strong></i>
</details>


<br>

## Method 1

```tex
【O(nlog(n))time∣O(1)space】
```

```java
package Greedy;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/05/03
 */
public class MinimumWaitingTime {
    public int minimumWaitingTime(int[] queries) {
        // Sort the array in non-decreasing order
        Arrays.sort(queries);

        // Initialize the last waiting time to 0
        int lastWaitingTime = 0;
        // Initialize the sum of waiting times to 0
        int sumOfWaitingTime = 0;

        // For each query except the first, add its duration to the last waiting time
        // and add the resulting waiting time to the sum of waiting times
        for(int i = 1; i < queries.length; i++){
            lastWaitingTime += queries[i - 1];
            sumOfWaitingTime += lastWaitingTime;
        }

        // Return the sum of waiting times
        return sumOfWaitingTime;
    }
}
```

