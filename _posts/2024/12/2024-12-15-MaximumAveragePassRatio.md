---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1792. Maximum Average Pass Ratio"
date: "2024-12-15"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array `classes`, where `classes[i] = [passi, totali]`. You know beforehand that in the `ith` class, there are `totali` total students, but only `passi` number of students will pass the exam.

- You are also given an integer `extraStudents`. There are another `extraStudents` brilliant students that are **guaranteed** to pass the exam of any class they are assigned to. You want to assign each of the `extraStudents` students to a class in a way that **maximizes** the **average** pass ratio across **all** the classes.

- The **pass ratio** of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The **average pass ratio** is the sum of pass ratios of all the classes divided by the number of the classes.

  Return *the **maximum** possible average pass ratio after assigning the* `extraStudents` *students.* Answers within 10-5` of the actual answer will be accepted.

**Example 1**

```
Input: classes = [[1,2],[3,5],[2,2]], extraStudents = 2
Output: 0.78333
Explanation: You can assign the two extra students to the first class. The average pass ratio will be equal to (3/4 + 3/5 + 2/2) / 3 = 0.78333.
```

**Example 2**

```
Input: classes = [[2,4],[3,9],[4,5],[2,10]], extraStudents = 4
Output: 0.53485
```

## Method 1

```tex
【O((n+k)log(n) time | O(n) space】
```

```java
package Leetcode.Greedy;

import java.util.Comparator;
import java.util.PriorityQueue;

/**
 * @author zhengxingxing
 * @date 2024/12/15
 */
public class MaximumAveragePassRatio {
    public static double maxAverageRatio(int[][] classes, int extraStudents) {
        int len = classes.length;

        // Custom comparator to prioritize classes based on "marginal gain" in pass ratio
        PriorityQueue<double[]> pq = new PriorityQueue<double[]>(
                new Comparator<double[]>(){
                    @Override
                    public int compare(double[] a, double[] b ){
                        // Calculate the improvement in pass ratio when adding one student
                        // For class `a`: (a[0]+1)/(a[1]+1) - a[0]/a[1]
                        // Example: If a class improves from 1/2 to 2/3:
                        // Original ratio: 1/2 = 0.5
                        // New ratio: 2/3 ≈ 0.667
                        // Gain: 0.667 - 0.5 = 0.167
                        double diffa = (a[0]+1)/(a[1]+1) - a[0]/a[1];
                        double diffb = (b[0]+1)/(b[1]+1) - b[0]/b[1];

                        // Comparison logic:
                        // 1. If the gain is equal, return 0
                        if (diffa == diffb) {
                            return 0;
                        }
                        // 2. If the gain for class `a` is greater than `b`, return -1
                        // PriorityQueue is a min-heap, so returning -1 means `a` has higher priority
                        // We want the class with the maximum gain to be at the front
                        return diffa > diffb ? -1 : 1;
                    }
                }
        );
        // Add all class information to the priority queue
        for (int i = 0; i < len; i++) {
            pq.offer(new double[] {
                    Double.valueOf(classes[i][0]),
                    Double.valueOf(classes[i][1])
            });
        }

        // Allocate additional students
        while (!pq.isEmpty() && extraStudents > 0) {
            double[] p = pq.poll(); // Remove the class with the highest marginal gain

            p[0] += 1; // Increment passed students
            p[1] += 1; // Increment total students

            pq.offer(p); // Re-add the updated class back to the queue
            extraStudents--;
        }

        // Calculate the final average pass ratio
        double res = 0;
        for (double[] p : pq) {
            res += p[0] / p[1];
        }

        // Return the average pass ratio
        return res / Double.valueOf(len);
    }

    public static void main(String[] args) {

        // Test case 1: Three classes, two extra students
        int[][] classes1 = {{1,2},{3,5},{2,2}};
        int extraStudents1 = 2;
        double result1 = maxAverageRatio(classes1, extraStudents1);
        System.out.println("Test case 1 result: " + result1);

        // Test case 2: Four classes, four extra students
        int[][] classes2 = {{2,4},{3,9},{4,5},{2,10}};
        int extraStudents2 = 4;
        double result2 = maxAverageRatio(classes2, extraStudents2);
        System.out.println("Test case 2 result: " + result2);

        // Boundary test case: Only one class
        int[][] classes3 = {{1,3}};
        int extraStudents3 = 1;
        double result3 = maxAverageRatio(classes3, extraStudents3);
        System.out.println("Test case 3 result: " + result3);
    }
}

```





