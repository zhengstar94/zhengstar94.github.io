---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1550. Three Consecutive Odds"
date: "2025-05-11"
tags: Easy
categories:
  - "LeetCode SlideWindow"
---


- Given an integer array `arr`, return `true` if there are three consecutive odd numbers in the array. Otherwise, return `false`.

**Example 1**

```
Input: arr = [2,6,4,1]
Output: false
Explanation: There are no three consecutive odds.
```

**Example 2**

```
Input: arr = [1,2,34,3,4,5,7,23,12]
Output: true
Explanation: [5,7,23] are three consecutive odds.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2025/05/11
 */
public class ThreeConsecutiveOdds {

    public static boolean threeConsecutiveOdds(int[] arr) {
        // Edge case: If array length is less than 3, it's impossible to have three consecutive elements
        if (arr.length < 3){
            return false;
        }

        // Counter to track the number of consecutive odd integers encountered
        int count = 0;

        // Iterate through each element in the array
        for (int num: arr) {
            // Check if the current number is odd (remainder when divided by 2 is 1)
            if (num % 2 == 1){
                // Increment the counter for consecutive odd numbers
                count++;
                // If we've found three consecutive odd numbers, return true
                if (count >= 3){
                    return true;
                }
            } else {
                // If we encounter an even number, reset the counter
                // as it breaks the consecutive sequence of odd numbers
                count = 0;
            }
        }

        // If we've traversed the entire array without finding three consecutive odd numbers, return false
        return false;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Array with no three consecutive odd numbers
        // Expected output: false
        int[] arr1 = {2, 6, 4, 1};
        System.out.println("Test Case 1 Result: " + threeConsecutiveOdds(arr1));  // Expected output: false

        // Test Case 2: Array with three consecutive odd numbers (5, 7, 23)
        // Expected output: true
        int[] arr2 = {1, 2, 34, 3, 4, 5, 7, 23, 12};
        System.out.println("Test Case 2 Result: " + threeConsecutiveOdds(arr2));  // Expected output: true

        // Test Case 3: Array with exactly three elements, all odd
        // Expected output: true
        int[] arr3 = {1, 3, 5};
        System.out.println("Test Case 3 Result: " + threeConsecutiveOdds(arr3));  // Expected output: true

        // Test Case 4: Array with consecutive odd numbers scattered throughout
        // Expected output: true (could be because of 7, 3, 1 or 3, 1, 9 or 1, 9, 5)
        int[] arr4 = {7, 3, 1, 9, 5};
        System.out.println("Test Case 4 Result: " + threeConsecutiveOdds(arr4));  // Expected output: true

        // Test Case 5: Array with only even numbers
        // Expected output: false
        int[] arr5 = {2, 4, 6};
        System.out.println("Test Case 5 Result: " + threeConsecutiveOdds(arr5));  // Expected output: false
    }
}
```





