---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1010. Pairs of Songs With Total Durations Divisible by 60"
date: "2024-11-25"
categories:
  - "LeetCode HashTable"
---

- You are given a list of songs where the `ith` song has a duration of `time[i]` seconds.
- Return *the number of pairs of songs for which their total duration in seconds is divisible by* `60`. Formally, we want the number of indices `i`, `j` such that `i < j` with `(time[i] + time[j]) % 60 == 0`.

**Example 1**

```
Input: time = [30,20,150,100,40]
Output: 3
Explanation: Three pairs have a total duration divisible by 60:
(time[0] = 30, time[2] = 150): total duration 180
(time[1] = 20, time[3] = 100): total duration 120
(time[1] = 20, time[4] = 40): total duration 60
```

**Example 2**

```
Input: time = [60,60,60]
Output: 3
Explanation: All three pairs have a total duration of 120, which is divisible by 60.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.HashTable;

/**
 * @author zhengxingxing
 * @date 2024/11/25
 */
public class NumPairsDivisibleBy60 {
    public static int numPairsDivisibleBy60(int[] time) {
        // Array to store the count of remainders when divided by 60
        int[] remainders = new int[60];
        // Counter for valid pairs
        int count = 0;

        for (int t: time) {
            // Calculate the remainder of current number when divided by 60
            int remainder = t % 60;

            // Calculate the complementary remainder
            // If remainder is 0, complement should be 0
            // If remainder is 30, complement should be 30
            // For all other cases, complement is (60 - remainder)
            // Using % 60 to handle the case when remainder is 0
            int complement = (60 - remainder) % 60;

            // Key logic explanation:
            // 1. remainders[complement] stores the count of numbers previously seen
            //    that have a remainder equal to complement when divided by 60
            // 2. Current number t can form a valid pair with any number that has
            //    a remainder of complement, as their sum will be divisible by 60
            // 3. Therefore, add remainders[complement] to count, representing
            //    the number of valid pairs that can be formed with current number
            count += remainders[complement];

            // Update the count of current remainder
            // This prepares for future numbers that might pair with this one
            remainders[remainder]++;
        }
        return count;
    }

    public static void main(String[] args) {
        // Test Case 1: Regular case with multiple possible pairs
        int[] test1 = {30,20,150,100,40};
        System.out.println("Test Case 1: [ 30,20,150,100,40 ] ");
        System.out.println("Expected Result: 3");
        System.out.println("Actual Result: " + numPairsDivisibleBy60(test1));
        System.out.println();

        // Test Case 2: All numbers are multiples of 60
        int[] test2 = {60,60,60};
        System.out.println("Test Case 2: [ 60,60,60 ] ");
        System.out.println("Expected Result: 3");
        System.out.println("Actual Result: " + numPairsDivisibleBy60(test2));
        System.out.println();

        // Test Case 3: No pairs sum up to multiple of 60
        int[] test3 = {1,2,3,4};
        System.out.println("Test Case 3: [ 1,2,3,4 ] ");
        System.out.println("Expected Result: 0");
        System.out.println("Actual Result: " + numPairsDivisibleBy60(test3));
        System.out.println();

        // Test Case 4: Contains 0 and multiples of 60
        int[] test4 = {0,60,120,180};
        System.out.println("Test Case 4: [ 0,60,120,180 ] ");
        System.out.println("Expected Result: 6");
        System.out.println("Actual Result: " + numPairsDivisibleBy60(test4));
        System.out.println();

        // Test Case 5: Testing complementary remainders
        int[] test5 = {10,50,20,40};  // 10+50=60, 20+40=60
        System.out.println("Test Case 5: [ 10,50,20,40 ] ");
        System.out.println("Expected Result: 2");
        System.out.println("Actual Result: " + numPairsDivisibleBy60(test5));
    }
}

```
