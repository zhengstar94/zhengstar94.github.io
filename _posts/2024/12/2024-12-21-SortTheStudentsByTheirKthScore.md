---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "2545. Sort the Students by Their Kth Score"
date: "2024-12-21"
tags: Easy
categories:
  - "LeetCode Array"
---


- There is a class with `m` students and `n` exams. You are given a **0-indexed** `m x n` integer matrix `score`, where each row represents one student and `score[i][j]` denotes the score the `ith` student got in the `jth` exam. The matrix `score` contains **distinct** integers only.
- You are also given an integer `k`. Sort the students (i.e., the rows of the matrix) by their scores in the `kth` (**0-indexed**) exam from the highest to the lowest.
- Return *the matrix after sorting it.*


**Example 1**

```
Input: score = [ [10,6,9,1],[7,5,11,2],[4,8,3,15 ] ], k = 2
Output: [ [7,5,11,2],[10,6,9,1],[4,8,3,15 ] ]
Explanation: In the above diagram, S denotes the student, while E denotes the exam.
- The student with index 1 scored 11 in exam 2, which is the highest score, so they got first place.
- The student with index 0 scored 9 in exam 2, which is the second highest score, so they got second place.
- The student with index 2 scored 3 in exam 2, which is the lowest score, so they got third place.
```

**Example 2**

```
Input: score = [ [ 3,4],[5,6 ] ], k = 0
Output: [ [ 5,6],[3,4 ] ]
Explanation: In the above diagram, S denotes the student, while E denotes the exam.
- The student with index 1 scored 5 in exam 0, which is the highest score, so they got first place.
- The student with index 0 scored 3 in exam 0, which is the lowest score, so they got second place.
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/21
 */
public class SortTheStudentsByTheirKthScore {
    public static int[][] sortTheStudents(int[][] score, int k) {
        // Sort the array in descending order based on the k-th column
        Arrays.sort(score, (a, b) -> b[k] - a[k]);
        return score;
    }

    public static void main(String[] args) {

        // Test Case 1
        System.out.println("Test Case 1:");
        int[][] score1 = { {10, 6, 9, 1}, {7, 5, 11, 2}, {4, 8, 3, 15 } };
        int k1 = 2;
        System.out.println("Original matrix:");
        printMatrix(score1);
        System.out.println("Sorted matrix by column " + k1 + ":");
        printMatrix(sortTheStudents(score1, k1));

        // Test Case 2
        System.out.println("Test Case 2:");
        int[][] score2 = { { 3, 4}, {5, 6 } };
        int k2 = 0;
        System.out.println("Original matrix:");
        printMatrix(score2);
        System.out.println("Sorted matrix by column " + k2 + ":");
        printMatrix(sortTheStudents(score2, k2));

        // Additional Test Case 3 (Edge case with single row)
        System.out.println("Test Case 3 (Single row):");
        int[][] score3 = { { 1, 2, 3 } };
        int k3 = 1;
        System.out.println("Original matrix:");
        printMatrix(score3);
        System.out.println("Sorted matrix by column " + k3 + ":");
        printMatrix(sortTheStudents(score3, k3));
    }

    private static void printMatrix(int[][] matrix) {
        for (int[] row : matrix) {
            System.out.println(Arrays.toString(row));
        }
        System.out.println();
    }
}

```





