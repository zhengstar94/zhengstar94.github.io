---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2094. Finding 3-Digit Even Numbers"
date: "2025-05-12"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given an integer array `digits`, where each element is a digit. The array may contain duplicates.

- You need to find **all** the **unique** integers that follow the given requirements:

  - The integer consists of the **concatenation** of **three** elements from `digits` in **any** arbitrary order.
  - The integer does not have **leading zeros**.
  - The integer is **even**.

- For example, if the given `digits` were `[1, 2, 3]`, integers `132` and `312` follow the requirements.

  Return *a **sorted** array of the unique integers.*

**Example 1**

```
Input: digits = [2,1,3,0]
Output: [102,120,130,132,210,230,302,310,312,320]
Explanation: All the possible integers that follow the requirements are in the output array. 
Notice that there are no odd integers or integers with leading zeros.
```

**Example 2**

```
Input: digits = [2,2,8,8,2]
Output: [222,228,282,288,822,828,882]
Explanation: The same digit can be used as many times as it appears in digits. 
In this example, the digit 8 is used twice each time in 288, 828, and 882. 
```

**Example 3**

```
Input: digits = [3,7,5]
Output: []
Explanation: No even integers can be formed using the given digits.
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/05/12
 */
public class Finding3DigitEvenNumbers {
    public static int[] findEvenNumbers(int[] digits) {
        // Create an array to count the frequency of each digit (0-9) in the input array
        // This allows us to track how many times each digit appears in the input
        int[] count = new int[10];
        for (int digit : digits) {
            count[digit]++;
        }

        // List to store the valid 3-digit even numbers we find
        List<Integer> result = new ArrayList<>();

        // Iterate through all possible 3-digit even numbers
        // Start from 100 (smallest 3-digit number)
        // End at 998 (largest 3-digit even number)
        // Increment by 2 to only check even numbers (numbers ending with 0,2,4,6,8)
        for (int num = 100; num <= 998; num += 2) {
            // Extract each digit from the current number
            int d1 = num / 100;         // Hundreds place (first digit)
            int d2 = (num / 10) % 10;   // Tens place (second digit)
            int d3 = num % 10;          // Ones place (third digit)

            // Temporarily decrement the count for each digit we want to use
            // This simulates "using" these digits to form our number
            count[d1]--;
            count[d2]--;
            count[d3]--;

            // Check if we can form this number with the available digits
            // For each digit, if count >= 0 after decrementing, it means:
            // 1. If count = 0: We've used exactly all occurrences of this digit
            // 2. If count > 0: We still have more occurrences of this digit available
            // 
            // If any count < 0, it means we tried to use more occurrences of a digit
            // than what's available in the original array, which is invalid
            if (count[d1] >= 0 && count[d2] >= 0 && count[d3] >= 0) {
                // If all counts are valid (>= 0), we can form this number
                // so add it to our result list
                result.add(num);
            }

            // Restore the counts for the next iteration
            // This is critical because we're only "simulating" using these digits
            // and need to reset for checking the next number
            count[d1]++;
            count[d2]++;
            count[d3]++;
        }

        // Convert the ArrayList to a primitive int array and return
        return result.stream().mapToInt(i -> i).toArray();
    }

    public static void main(String[] args) {
        // Test case 1: Mixed digits with an even digit and zero
        int[] digits1 = {2, 1, 3, 0};
        System.out.println("Test case 1 result: " + Arrays.toString(findEvenNumbers(digits1)));

        // Test case 2: Repeated digits
        int[] digits2 = {2, 2, 8, 8, 2};
        System.out.println("Test case 2 result: " + Arrays.toString(findEvenNumbers(digits2)));

        // Test case 3: Only odd digits, no even numbers possible
        int[] digits3 = {3, 7, 5};
        System.out.println("Test case 3 result: " + Arrays.toString(findEvenNumbers(digits3)));
    }
}
```





