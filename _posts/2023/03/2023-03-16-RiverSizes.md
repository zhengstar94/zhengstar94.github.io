---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "River Sizes"
date: "2023-03-16"
categories:
  - "Algorithm Graphs"
---

# River Sizes [Medium]

- You're given a two-dimensional array (a matrix)of potentially unequal height and width containing only `0` and `1`s .Each `0` represents land, and each `1` represents part of a river. A river consists of any number of `1`s that are either horizontally or vertically adjacent (but not diagonally adjacent). The number of adjacent `1`s forming a river determine its size.

- Note that a river can twist.In other words, it doesn't have to be a straight vertical line or a straight horizontal line; it can be L-shaped, for example.

- Write a function that returns an array of the sizes of all rivers represented in the input matrix. The sizes don't need to be in any particular order.




**Sample Input **

````
matrix = [
[1, 0, 0, 1, 0],
[1, 0, 1, 0, 0],
[0, 0, 1, 0, 1],
[1, 0, 1, 0, 1],
[1, 0, 1, 1, 0],
]
````

**Sample Output **

```
[1,2,2,2,5] //The numbers could be ordered differently.

// The rivers can be clearly seen here:
// [
// 	[1,  ,  , 1,  ],
// 	[1,  , 1,  ,  ],
// 	[ ,  , 1,  , 1],
// 	[1,  , 1,  , 1],
// 	[1,  , 1, 1,  ],
// ]

```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Since you must return the sizes of rivers,which consist of horizontally and vertically adjacent 1s in the input matrix,you must somehow keep track of groups of neighboring 1s as you traverse the matrix. Try treating the matrix as a graph, where each element in the matrix is a node in the graph with up to 4 neighboring nodes (above, below, to the left, and to the right), and traverse it using a popular graph-traversal algorithm like Depth-first Search or Breadth-first Search. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> By traversing the matrix using DFS or BFS as mentioned in Hint #1, any time that you encounter a 1 you can traverse the entire river that this 1 is a part of (and keep track of its size)by simply iterating through the given node's neighboring nodes and their own neighboring nodes so long as the nodes are 1s. </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Naturally, many nodes in the graph mentioned in Hint #1 will have overlapping neighboring nodes, and as you traverse the matrix, you will undoubtedly encounter nodes that you have previously visited.In order to prevent mistakenly calculating the same river's size multiple times and to avoid doing needless computational work, try keeping track of every node that you visit in an auxiliary data structure and only performing important computations on unvisited nodes. What data structure would be ideal here? </strong></i>
</details>

<br>

## Method 1

```tex
【O(nm)time∣O(nm)space】
```

```java
package Graphs;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/03/07
 */
public class RiverSizes {
    public static List<Integer> riverSizes(int[][] matrix) {
        // Create an ArrayList to store the size of each river
        List<Integer> sizes = new ArrayList<>();
        // Get the number of rows and columns of the matrix
        int rows = matrix.length;
        int cols = matrix[0].length;

        // Use a double loop to traverse the entire matrix
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // If the current location is the starting point of a river
                if (matrix[i][j] == 1) {
                    // Use DFS to traverse the entire river and calculate its size
                    int size = traverse(matrix, i, j);
                    // Add the size of the current river to the ArrayList
                    sizes.add(size);
                }
            }
        }

        // Return to the size of all rivers
        return sizes;
    }

    /**
     * Use DFS to traverse the entire river and calculate its size
     * @param matrix
     * @param row
     * @param col
     * @return
     */
    private static int traverse(int[][] matrix, int row, int col) {
        // Initialize the size of the current river to 0
        int size = 0;
        // Get the number of rows and columns of the matrix
        int rows = matrix.length;
        int cols = matrix[0].length;

        // If the current position is outside the range of the matrix,
        // or if the current position is not a point of a river, 0 is returned directly.
        if (row < 0 || row >= rows || col < 0 || col >= cols || matrix[row][col] == 0) {
            return size;
        }

        // Marks the current location as a visited point to prevent repeated traversal
        matrix[row][col] = 0;
        // The current size of the river plus one
        size++;

        // DFS traversing the four adjacent points of the current point

        // up
        size += traverse(matrix, row - 1, col);
        // down
        size += traverse(matrix, row + 1, col);
        // left
        size += traverse(matrix, row, col - 1);
        // right
        size += traverse(matrix, row, col + 1);

        // Returns the size of the current river
        return size;
    }
}

```