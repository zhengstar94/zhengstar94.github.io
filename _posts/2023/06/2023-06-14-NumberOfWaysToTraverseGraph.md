---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Number Of Ways To Traverse Graph"
date: "2023-06-14"
categories:
  - "Algorithm Dynamic"
---


# Number Of Ways To Traverse Graph [Medium]

- You're given two positive integers representing the width and height of a grid-shaped, rectangular graph. Write a function that returns the number of ways to reach the bottom right corner of the graph when starting at the top left corner. Each move you take must either go down or right. In other words, you can never move up or left in the graph.
- For example, given the graph illustrated below, with width = 2 and height = 3, there are three ways to reach the bottom right corner when starting at the top left corner:

```
 _ _
|_|_|
|_|_|
|_|_|

Down, Down, Right
Right, Down, Down
Down, Right, Down
```

**Sample Input**

```
width = 4
height = 3
```

**Sample Output**

```
10
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Think recursively. How many positions in the graph can access the bottom right corner of the graph? In other words, what positions do you need to reach before you can reach the bottom right corner? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>The number of ways to reach any position in the graph is equal to the number of ways to reach the position directly above it plus the number of ways to reach the position directly to its left. This is because you can only travel down and right. </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Using the information in Hints #1 and #2, can you come up with an efficient way to solve this problem that doesn't repeatedly perform the same work? What does a dynamic-programming implementation look like? </strong></i>
</details>

<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>To efficiency solve this problem, simply loop through the entire graph, column by column, row by row, and calculate the number of ways to reach each position. If you're on the top or left edge of the graph, there's only one way to reach your position. If you're anywhere else in the graph, the number of ways to reach your position is the number of ways to reach the position directly above it plus the number of ways to reach the position directly to its left (which you've already calculated and should be storing). Every time you calculate the number of ways to reach a position, store the answer so that you can use it later in the calculation of other positions. </strong></i>
</details>

<br>

## Method 1

```tex
【O(nm)time∣O(nm)space】
```

```tex
1.\ Where\ n\ is\ the\ width\ of\ the\ graph\ and\ m\ is\ the\ height\\
```

```java
public class Main {

    public static void main(String[] args) {
        System.out.println(numberOfWaysToTraverseGraph(4, 3));
    }

    public static int numberOfWaysToTraverseGraph(int width, int height) {
        // Define a 2D array to store the number of ways to reach each cell
        int[][] numOfPaths = new int[height + 1][width + 1];

        // Iterate through each cell of the grid
        for (int i = 1; i <= height; i++) {
            for (int j = 1; j <= width; j++) {
                // For the first row and the first column, there is only one way
                // to reach each cell -- moving rightward (for the first row) or downward (for the first column)
                if (i == 1 || j == 1) {
                    numOfPaths[i][j] = 1;
                } 
                // For the rest of the cells, the number of ways to reach it
                // is the sum of the number of ways to reach the cell above it and the cell left to it
                else {
                    numOfPaths[i][j] = numOfPaths[i - 1][j] + numOfPaths[i][j - 1];
                }
            }
        }

        // Return the number of ways to reach the bottom-right most cell
        return numOfPaths[height][width];
    }
}
```



## Method 2

```tex
【O(n + m)time∣O(1)space】
```

```tex
1.\ Where\ n\ is\ the\ width\ of\ the\ graph\ and\ m\ is\ the\ height\\
```

```java
package Dynamic;

/**
 * @author zhengstars
 * @date 2024/01/09
 */
public class NumberOfWaysToTraverseGraph {
    // The function to calculate the number of unique paths from top-left to bottom-right of the grid
    public static int numberOfWaysToTraverseGraph(int width, int height) {
        // Since we know that total number of movements will be width+height-2
        // and we just need to choose the number of down movements or the number of right movements
        // we can use combination formula to calculate the result
        return combination(width+height-2, height-1);
    }

    // The function to calculate the combination of n elements taken k at a time
    public static int combination(int n, int k) {
        long result = 1;
        // Here we calculate the fraction n*(n-1)*...(n-k+1) / k*(k-1)*...*2*1
        for(int i=1; i<=k; i++) {
            // The numerator will be multiplied with one factor of n each time, and the denominator will be multiplied by one factor of k
            result = result * (n-i+1) / i;
        }
        return (int)result;
    }

    public static void main(String[] args) {
        System.out.println(numberOfWaysToTraverseGraph(4, 3));
    }
}

```







