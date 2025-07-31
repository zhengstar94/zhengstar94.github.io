---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "118. Pascal's Triangle"
date: "2025-08-01"
tags: Easy
categories:
  - "LeetCode Math"
---

- Given an integer `numRows`, return the first numRows of **Pascal's triangle**.
- In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

**Example 1**

```
Input: numRows = 5
Output: [ [ 1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1 ] ]
```

**Example 2**

```
Input: numRows = 1
Output: [ [ 1 ] ]
```

## Method 1

```tex
【O(numRows^2) time | O(numRows^2) space】
```

```java
package Leetcode.Math;

import java.util.ArrayList;
import java.util.List;

/**
 * @Author zhengxingxing
 * @Date 2025/08/01
 */
public class PascalTriangle {

    public static List<List<Integer>> generate(int numRows) {
        // This list will store all rows of Pascal's Triangle.
        List<List<Integer>> result = new ArrayList<>();

        // Loop through each row index from 0 to numRows - 1.
        for (int i = 0; i < numRows; i++) {
            // Create a new list to represent the current row.
            List<Integer> row  = new ArrayList<>();

            // Loop through each element in the current row.
            for (int j = 0; j <= i; j++) {
                // The first and last elements of each row are always 1.
                if (j == 0 || j == i){
                    row.add(1);
                } else {
                    // Each inner element is the sum of the two elements directly above it
                    // from the previous row: result.get(i - 1).get(j - 1) and result.get(i - 1).get(j)
                    row.add(result.get(i - 1).get(j - 1) + result.get(i - 1).get(j));
                }
            }
            // Add the completed row to the result list.
            result.add(row);
        }
        // Return the complete Pascal's Triangle.
        return result;
    }

    public static void main(String[] args) {
        int numRows1 = 5;
        int numRows2 = 1;
        // Test case 1: Generate 5 rows of Pascal's Triangle.
        System.out.println("numRows = 5: " + generate(numRows1));
        // Test case 2: Generate 1 row of Pascal's Triangle.
        System.out.println("numRows = 1: " + generate(numRows2));
    }
}

```





