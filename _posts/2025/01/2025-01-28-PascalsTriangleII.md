---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "119. Pascal's Triangle II"
date: "2025-01-28"
tags: Easy
categories:
  - "LeetCode Math"
---


- Given an integer `rowIndex`, return the `rowIndexth` (**0-indexed**) row of the **Pascal's triangle**.
- In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

**Example 1**

```
Input: rowIndex = 3
Output: [1,3,3,1]
```

**Example 2**

```
Input: rowIndex = 0
Output: [1]
```

**Example 3**

```
Input: rowIndex = 1
Output: [1,1]
```

## Method 1

```tex
【O(rowIndex) time | O(rowIndex) space】
```

```java
package Leetcode.Math;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/28
 */
public class PascalsTriangleII {

    public static List<Integer> getRow(int rowIndex) {
        // Initialize a list to store the final result.
        List<Integer> result = new ArrayList<>();

        // Create an array to store the current row values.
        // The size of the array is rowIndex + 1 because the row is 0-indexed.
        int[] row = new int[rowIndex + 1];

        // The first element of every row in Pascal's Triangle is always 1.
        row[0] = 1;

        // Loop to calculate each element in the row.
        // Start from the second element (i=1) because the first element is already set to 1.
        for (int i = 1; i <= rowIndex; i++) {
            // Get the previous element in the row.
            // This is equivalent to C(n, k-1) in the combination formula.
            long prev = row[i - 1];

            // Calculate the current element using the combination formula:
            // C(n, k) = C(n, k-1) * (n - k + 1) / k
            // Here:
            // - n is the rowIndex.
            // - k is the current position in the row (i).
            // - (rowIndex - i + 1) is the numerator part of the formula.
            // - i is the denominator part of the formula.
            long current = prev * (rowIndex - i + 1) / i;

            // Store the calculated value in the current position of the row.
            row[i] = (int) current;
        }

        // Convert the array to a list for the final result.
        for (int num : row) {
            result.add(num);
        }

        // Return the generated row.
        return result;
    }


    public static void main(String[] args) {
        // Test case 1: rowIndex = 3
        System.out.println("rowIndex = 3: " + getRow(3));  // Expected output: [1, 3, 3, 1]

        // Test case 2: rowIndex = 0
        System.out.println("rowIndex = 0: " + getRow(0));  // Expected output: [1]

        // Test case 3: rowIndex = 1
        System.out.println("rowIndex = 1: " + getRow(1));  // Expected output: [1, 1]

        // Additional test case: rowIndex = 4
        System.out.println("rowIndex = 4: " + getRow(4));  // Expected output: [1, 4, 6, 4, 1]
    }
}
```











