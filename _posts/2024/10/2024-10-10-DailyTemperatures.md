---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "739.Daily Temperatures"
date: "2024-10-10"
categories:
  - "LeetCode Stack"
---

- Given an array of integers `temperatures` represents the daily temperatures, return *an array* `answer` *such that* `answer[i]` *is the number of days you have to wait after the* `ith` *day to get a warmer temperature*. If there is no future day for which this is possible, keep `answer[i] == 0` instead.


**Example 1**

```
Input: temperatures = [ 73,74,75,71,69,72,76,73]
Output: [ 1,1,4,2,1,1,0,0]
```

**Example 2**

```
Input: temperatures = [ 30,40,50,60]
Output: [ 1,1,1,0]
```

**Example 2**

```
Input: temperatures = [30,60,90]
Output: [1,1,0]
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Deque;
import java.util.Stack;

/**
 * @author zhengstars
 * @date 2024/10/10
 */
public class DailyTemperatures {
    public static int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        // Initialize the result array with zeros
        int[] answer = new int[n];
        // Use a stack to keep track of indices of temperatures
        Deque<Integer> stack = new ArrayDeque<>();

        // Iterate through each temperature
        for (int i = 0; i < n; i++) {
            // While the stack is not empty and the current temperature is warmer
            // than the temperature at the top of the stack
            while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
                // Pop the index of the cooler day
                int prevIndex = stack.pop();
                // Calculate the number of days to wait for a warmer temperature
                // i - prevIndex represents the number of days between the current day
                // and the day at prevIndex
                // This is the key operation: it calculates how many days you need to wait
                // from the day at prevIndex to get to a warmer day (the current day i)
                answer[prevIndex] = i - prevIndex;
            }
            // Push the current day's index onto the stack
            stack.push(i);
        }

        return answer;
    }

    // Test case
    public static void main(String[] args) {
        int[] temperatures = { 73, 74, 75, 71, 69, 72, 76, 73};
        int[] result = dailyTemperatures(temperatures);
        System.out.println("Input temperatures: " + Arrays.toString(temperatures));
        System.out.println("Days to wait for warmer temperature: " + Arrays.toString(result));
        // Expected output: [ 1, 1, 4, 2, 1, 1, 0, 0]
    }
}

```





