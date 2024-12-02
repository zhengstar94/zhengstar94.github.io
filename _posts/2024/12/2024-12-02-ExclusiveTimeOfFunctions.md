---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "636. Exclusive Time of Functions"
date: "2024-12-02"
tags: Medium
categories:
  - "LeetCode Stack"
---


- On a **single-threaded** CPU, we execute a program containing `n` functions. Each function has a unique ID between `0` and `n-1`.
- Function calls are **stored in a [call stack](https://en.wikipedia.org/wiki/Call_stack)**: when a function call starts, its ID is pushed onto the stack, and when a function call ends, its ID is popped off the stack. The function whose ID is at the top of the stack is **the current function being executed**. Each time a function starts or ends, we write a log with the ID, whether it started or ended, and the timestamp.
- You are given a list `logs`, where `logs[i]` represents the `ith` log message formatted as a string `"{function_id}:{"start" | "end"}:{timestamp}"`. For example, `"0:start:3"` means a function call with function ID `0` **started at the beginning** of timestamp `3`, and `"1:end:2"` means a function call with function ID `1` **ended at the end** of timestamp `2`. Note that a function can be called **multiple times, possibly recursively**.
- A function's **exclusive time** is the sum of execution times for all function calls in the program. For example, if a function is called twice, one call executing for `2` time units and another call executing for `1` time unit, the **exclusive time** is `2 + 1 = 3`.
- Return *the **exclusive time** of each function in an array, where the value at the* `ith` *index represents the exclusive time for the function with ID* `i`.

**Example 1**

```
Input: n = 2, logs = ["0:start:0","1:start:2","1:end:5","0:end:6"]
Output: [3,4]
Explanation:
Function 0 starts at the beginning of time 0, then it executes 2 for units of time and reaches the end of time 1.
Function 1 starts at the beginning of time 2, executes for 4 units of time, and ends at the end of time 5.
Function 0 resumes execution at the beginning of time 6 and executes for 1 unit of time.
So function 0 spends 2 + 1 = 3 units of total time executing, and function 1 spends 4 units of total time executing.
```

**Example 2**

```
Input: n = 1, logs = ["0:start:0","0:start:2","0:end:5","0:start:6","0:end:6","0:end:7"]
Output: [8]
Explanation:
Function 0 starts at the beginning of time 0, executes for 2 units of time, and recursively calls itself.
Function 0 (recursive call) starts at the beginning of time 2 and executes for 4 units of time.
Function 0 (initial call) resumes execution then immediately calls itself again.
Function 0 (2nd recursive call) starts at the beginning of time 6 and executes for 1 unit of time.
Function 0 (initial call) resumes execution at the beginning of time 7 and executes for 1 unit of time.
So function 0 spends 2 + 4 + 1 + 1 = 8 units of total time executing.
```

**Example 3**

```
Input: n = 2, logs = ["0:start:0","0:start:2","0:end:5","1:start:6","1:end:6","0:end:7"]
Output: [7,1]
Explanation:
Function 0 starts at the beginning of time 0, executes for 2 units of time, and recursively calls itself.
Function 0 (recursive call) starts at the beginning of time 2 and executes for 4 units of time.
Function 0 (initial call) resumes execution then immediately calls function 1.
Function 1 starts at the beginning of time 6, executes 1 unit of time, and ends at the end of time 6.
Function 0 resumes execution at the beginning of time 6 and executes for 2 units of time.
So function 0 spends 2 + 4 + 1 = 7 units of total time executing, and function 1 spends 1 unit of total time executing.
```

## Method 1

```tex
【O(L) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.Arrays;
import java.util.List;
import java.util.Stack;

/**
 * @author zhengxingxing
 * @date 2024/12/02
 */
public class ExclusiveTimeOfFunctions {
    public static int[] exclusiveTime(int n, List<String> logs) {
        int[] result = new int[n]; // Initialize the result array to store the exclusive time for each function

        Stack<Integer> stack = new Stack<>(); // Stack to simulate the call stack of functions

        int prevTime = 0; // Variable to keep track of the previous timestamp for calculating time intervals

        // Loop through each log entry
        for (String log : logs) {
            String[] parts = log.split(":"); // Split each log entry into parts
            int id = Integer.parseInt(parts[0]); // Get the function ID from the log entry
            String type = parts[1]; // Get the type of the log (either "start" or "end")
            int timestamp = Integer.parseInt(parts[2]); // Get the timestamp from the log entry

            if (type.equals("start")) { // If the log indicates the start of a function
                // Before pushing the new function onto the stack, update the execution time of the function currently at the top of the stack
                if (!stack.isEmpty()) {
                    result[stack.peek()] += timestamp - prevTime; // Update the exclusive time of the function currently on top of the stack
                }

                stack.push(id); // Push the current function's ID onto the stack (this function is now running)
                prevTime = timestamp; // Update prevTime to the current timestamp, as we now know when the function started
            } else { // If the log indicates the end of a function
                // Pop the current function from the stack (this function has finished execution)
                result[stack.pop()] += timestamp - prevTime + 1; // Calculate the exclusive time of the function that has finished and update the result array
                prevTime = timestamp + 1; // Update prevTime to the next timestamp after the current function ends, for the next operation
            }

        }
        // Return the result array, containing the exclusive times for all functions
        return result; 
    }

    public static void main(String[] args) {
        // Example 1:
        List<String> logs1 = Arrays.asList("0:start:0","1:start:2","1:end:5","0:end:6");
        // Function 0 runs for 3 units of time (from 0 to 1 and from 6 to 7), and function 1 runs for 4 units of time (from 2 to 5)
        System.out.println(Arrays.toString(exclusiveTime(2, logs1))); // Output: [3, 4]

        // Example 2:
        List<String> logs2 = Arrays.asList("0:start:0","0:start:2","0:end:5","0:start:6","0:end:6","0:end:7");
        // Function 0 is recursively called. It runs for 2 units of time, then calls itself, runs for 4 units, and then continues
        System.out.println(Arrays.toString(exclusiveTime(1, logs2))); // Output: [8]

        // Example 3:
        List<String> logs3 = Arrays.asList("0:start:0","0:start:2","0:end:5","1:start:6","1:end:6","0:end:7");
        // Function 0 runs for 7 units of time (from 0 to 5, then from 6 to 7), and function 1 runs for 1 unit of time (from 6 to 6)
        System.out.println(Arrays.toString(exclusiveTime(2, logs3))); // Output: [7, 1]
    }
}

```





