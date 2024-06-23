---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Minimum Passes Of Matrix"
date: "2023-03-20"
categories:
  - "Algorithm Graphs"
---

# Minimum Passes Of Matrix [Medium]

- Write a function that takes in an integer matrix of potentially unequal height and width and returns the minimum number of passes required to convert all negative integers in the matrix to positive integers.

- A negative integer in the matrix can only be converted to a positive integer if one or more of its adjacent elements is positive. An adjacent element is an element that is to the left, to the right, above, or below the current element in the matrix. Converting a negative to a positive simply involves multiplying it by `-1`.

- Note that the `0` value is neither positive nor negative,  meaning that a can't convert an adjacent negative to a positive.

- A single pass through the matrix involves converting all the negative integers that can be converted at a particular point in time. For example, consider the following input matrix:

  ```
  [
    [0,-2,-1],
    [-5,2,0],
    [-6,-2,0]
  ]
  ```

- After a first pass, only 3 values can be converted to positives:

  ```
  [
    [0, 2, -1],
    [5, 2, 0],
    [-6, 2, 0],
  ]
  ```

- After a second pass, the remaining negative values can all be converted to positives:

  ```
  [
    [0,2,1],
    [5,2,0],
    [6,2,0]
  ]
  ```

- Note that the input matrix will always contain at least one element.If the negative integers in the input matrix can't all be converted to positives, regardless of how many passes are run, your function should return `1`.

**Sample Input**

```
matrix = [
	[0,-1,-3,2,0],
	[1,-2,-5,-1,-3],
	[3,0,0,-4,-1]
]
```

**Sample Output**

```
3
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> The brute-force approach to solving this problem is to simply iterate through the entire matrix, find all positive values,and change their negative neighbors to positive. You then repeat this process until no more negative neighbors exist. This approach works,but it doesn't run in an optimal time complexity;can you think of a another way to solve this? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The approach discussed in Hint #1 has you look at the same elements in the matrix multiple times. How can you ensure that you never process the same element more than once? </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Once a positive value has been found and you change its neighbors to positives,  this positive value can no longer lead to the conversion of any more negative values. Instead, its neighbors (that you just changed to positives)have the possibility of changing their own neighbors to positives. After you change a negative value to positive, you should store its position so that you can check if it can flip any of its neighbors in the next pass of the matrix.Can something similar to a breadth-first search help you do this? </strong></i>
</details>


<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> You can solve this problem in `0(w*h)`time, where `w` and `h` are the width and height of the matrix, by implementing a breadth-first search,starting from all the positive-value positions in the array. Initialize a queue that stores the positions of all positive values, iterate through the queue, dequeue elements out, and consider all of their neighbors.If any of their neighbors are negative, change them to positive, and store their positions in a secondary queue. Once the first queue is empty,increment your number of passes, and iterate through the second queue you created (the one with the positions of negatives that were changed to positives). Repeat this process until no values are converted during a pass. </strong></i>
</details>


<br>



<details> <summary><b>Hint 5</b></summary>
    <br>
    <i><strong> The approach discussed in Hint #4 can work using either one or two queues. If you decide to use only one queue, you'll need to differentiate the values that were already positive when the current pass started from the values that were changed to positive during the current pass. See the Conceptual Overview section of this question's vided explanation for a more in-depth explanation. </strong></i>
</details>

<br>

## Method 1

```tex
【O(mn)time∣O(mn)space】
```

```java
package Graphs;

import java.util.LinkedList;
import java.util.Queue;

/**
 * @author zhengstars
 * @date 2023/03/18
 */
public class MinimumPassesOfMatrix2 {
    public static int minimumPassesOfMatrix(int[][] matrix) {
        // Get the number of passes required to convert all negatives to positives
        int passes = getPasses(matrix);
        // If it is impossible to convert all negatives, return -1
        return passes >= 0 ? passes : -1;
    }

    private static int getPasses(int[][] matrix) {
        // Create a queue to store positive numbers that can be used to convert negatives
        Queue<int[]> positives = new LinkedList<>();
        // Count the number of negative numbers in the matrix
        int negatives = 0;

        // Iterate through all elements of the matrix
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                // If the element is negative, increment the count of negative numbers
                if (matrix[i][j] < 0) {
                    negatives++;
                } else if (matrix[i][j] > 0) {
                    // If the element is positive, add it to the queue of positives
                    positives.add(new int[] { i, j });
                }
            }
        }

        // Initialize the number of passes required to 0
        int passes = 0;

        // Use BFS to find the minimum number of passes required to convert all negatives to positives
        while (!positives.isEmpty()) {
            // Get the number of elements in the queue
            int size = positives.size();
            // For each element in the queue, check if it can convert neighboring negatives
            while (size-- > 0) {
                // Get the current positive element
                int[] curr = positives.poll();
                int row = curr[0], col = curr[1];
                // Define the directions to check for neighboring elements
                int[][] directions = { { -1, 0 }, { 0, -1 }, { 1, 0 }, { 0, 1 } };
                for (int[] dir : directions) {
                    int newRow = row + dir[0], newCol = col + dir[1];
                    // Check if the neighboring element is out of bounds
                    if (newRow < 0 || newRow >= matrix.length || newCol < 0 || newCol >= matrix[0].length) {
                        continue;
                    }
                    // Check if the neighboring element is negative
                    if (matrix[newRow][newCol] < 0) {
                        // Convert the negative element to positive by multiplying by -1
                        negatives--;
                        if (negatives == 0) {
                            // If all negatives have been converted, return the number of passes
                            return passes + 1;
                        }
                        matrix[newRow][newCol] *= -1;
                        // Add the newly converted positive element to the queue
                        positives.add(new int[] { newRow, newCol });
                    }
                }
            }
            // Increment the number of passes required
            passes++;
        }

        // If it is impossible to convert all negatives to positives, return -1
        return negatives == 0 ? passes : -1;
    }

}

```

