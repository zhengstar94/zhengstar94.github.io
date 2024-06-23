---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Count Squares"
date: "2023-01-23"
categories:
  - "Algorithm Array"
---

# Count Squares [Hard]

- Write a function that takes in a list of Cartesian coordinates (i.e., (x, y) coordinates)and returns the number of squares that can be formed by these coordinates.
- A square must have its four corners amongst the coordinates in order to be counted. A single coordinate can be used as a corner for multiple different squares.
- You can also assume that no coordinate will be farther than 100 units from the origin.

**Sample Input**

```
points = [
[1, 1], 
[0, 0], 
[-4, 2], 
[-2, -1], 
[0, 1],
[1, 0], 
[-1, 4]
]
```

**Sample Output**

```
	// [1, 1], [0, 0], [0, 1], and[1, 0]makes a square,
2 // as does[1, 1], [-4, 2], [-2, -1], and[-1, 4]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Given any two points,there are exactly three pairs of points that would make a square.</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> If two points are assumed to be diagonally across from each other in a square, there is only one pair of points that would complete the square. </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> All four points of a square will always be equidistant from the midpoint.</strong></i>
</details>


<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> The slopes of the two diagonals of a square are always negative reciprocals of each other. </strong></i>
</details>


<br>

## Method 1

```tex
【O(n^2)time∣O(n)space】
```

```java
package Array;

import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2023/05/13
 */
public class countSquares {
    public static int countSquares(int[][] coords) {
        // create a hash set to store all the points
        Set<Integer> set = new HashSet<>();
        // used for counting the number of squares
        int count = 0;
        for (int[] coord : coords) {
            // store the coordinates of all points into the hash set
            set.add(coord[0] * 10000 + coord[1]);
        }
        // loop through all pairs of coordinates to find four points that form a square
        for (int i = 0; i < coords.length; i++) {
            // set x1 and y1 to the x and y coordinates of the first point
            int x1 = coords[i][0];
            int y1 = coords[i][1];
            for (int j = i + 1; j < coords.length; j++) {
                // set x2 and y2 to the x and y coordinates of the second point
                int x2 = coords[j][0];
                int y2 = coords[j][1];
                // calculate the difference in x and y coordinates between the two points
                int dx = x2 - x1;
                int dy = y2 - y1;
                // calculate the coordinates of the third point (x3, y3) by adding the perpendicular to (x2, y2)
                int x3 = x2 + dy;
                int y3 = y2 - dx;
                // calculate the coordinates of the fourth point (x4, y4) by adding the perpendicular to (x1, y1)
                int x4 = x1 + dy;
                int y4 = y1 - dx;

                // check if the third and fourth points exist in the set of coordinates
                // if so, it means all four points form a square, and we increment the count
                if (set.contains(x3 * 10000 + y3) && set.contains(x4 * 10000 + y4)) {
                    count++;
                }
            }
        }

        // each square is counted twice, so we need to divide by 2
        return count / 2;
    }

    public static void main(String[] args) {
        int[][] coordinates = { {1,1}, {0,0}, {-4,2}, {-2,-1}, {0,1}, {1,0},{-1,4}};
        int result = countSquares(coordinates);
        System.out.println(result);
    }
}

```

