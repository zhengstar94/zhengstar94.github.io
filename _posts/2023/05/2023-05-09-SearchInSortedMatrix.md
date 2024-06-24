---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Search In Sorted Matrix"
date: "2023-05-09"
categories:
  - "Algorithm Searching"
---

# Search In Sorted Matrix [Medium]

- You're given a two-dimensional array (a matrix) of distinct integers and a target integer. Each row in the matrix is sorted, and each column is also sorted; the matrix doesn't necessarily have the same height and width.

- Write a function that returns an array of the row and column indices of the target integer if it's contained in the matrix, otherwise [-1, -1].

  ## 

**Sample Input **

```
matrix = [
            [1, 4, 7, 12, 15, 1000],
            [2, 5, 19, 31, 32, 1001],
            [3, 8, 24, 33, 35, 1002],
            [40, 41, 42, 44, 45, 1003],
            [99, 100, 103, 106, 128, 1004],
          ]
target = 44
```

**Sample Output **

```
[3, 3]
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Pick any number in the matrix and compare it to the target number. If this number is bigger than the target number, what does that tell you about all of the other numbers in this number's row and this number's column? What about if this number is smaller than the target number? </strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Try starting at the top right corner of the matrix, comparing the number there to the target number, and using whatever you gathered from Hint #1 to figure out what number to compare next if the top right number isn't equal to the target number. Continue until you find the target number or until you get past the extremities of the matrix. </strong></i>
</details>







<br>



## Method 1

```tex
【O(n+m)time∣O(1)space】
```

```java
package Searching;

/**
 * @author zhengstars
 * @date 2023/06/08
 */
public class SearchInSortedMatrix {
    public static int[] searchInSortedMatrix(int[][] matrix, int target) {
        // Start from the top row
        int row = 0;
        // Start from the rightmost column
        int col = matrix[0].length - 1;

        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) {
                // If the current element is equal to the target, return the indices
                return new int[]{row, col};
            } else if (matrix[row][col] > target) {
                // If the current element is greater than the target, move to the left column
                col--;
            } else {
                // If the current element is less than the target, move to the next row
                row++;
            }
        }

        // If the target is not found, return [-1, -1]
        return new int[]{-1, -1};
    }

    public static void main(String[] args) {
        int[][] matrix = {
                {1, 4, 7, 12, 15, 1000},
                {2, 5, 19, 31, 32, 1001},
                {3, 8, 24, 33, 35, 1002},
                {40, 41, 42, 44, 45, 1003},
                {99, 100, 103, 106, 128, 1004}
        };
        int target = 44;
        int[] result = searchInSortedMatrix(matrix, target);
        System.out.println("[" + result[0] + ", " + result[1] + "]");
    }
}

```

