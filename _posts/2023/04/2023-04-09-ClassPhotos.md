---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Class Photos"
date: "2023-04-09"
categories:
  - "Algorithm Greedy"
---

# Class Photos [Easy]

- It's photo day at the local school, and you're the photographer assigned to take class photos. The class that you'll be photographing has an even number of students, and all these students are wearing red or blue shirts. In fact, exactly half of the class is wearing red shirts, and the other half is wearing blue shirts. You're responsible for arranging the students in two rows before taking the photo. Each row should contain the same number of the students and should adhere to the following guidelines:
  - All students wearing red shirts must be in the same row.
  - All students wearing blue shirts must be in the same row.
  - Each student in the back row must be strictly taller than the student directly in front of them in the front row.
- You're given two input arrays:one containing the heights of all the students with red shirts and another one containing the heights of all the students with blue shirts. These arrays will always have the same length, and each height will be a positive integer. Write a function that returns whether or not a class photo that follows the stated guidelines can be taken.
- Note:you can assume that each class has at least 2 students.

**Sample Input**

```
redshirtHeights = [5,8,1,3,4] 
blueshirtHeights = [6,9,2,4,5]
```

**Sample Output**

```
true // Place all students with blue shirts in the back row.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Start by determining which row will have the students wearing blue shirts and which row will have the students wearing red shirts. Once you know this, how can you determine if it's possible to take the photo? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The shirt color of the tallest student will determine which students need to be placed in the back row. The tallest student can't be placed in the front row because there's no student taller than them who can be placed behind them. </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Once you know which students should be placed in each row, you can simply check if each student in the back row can be paired with a student in the front row who is shorter than them. If you can't find a satisfactory pairing for every student in the back row, then you can't take the photo. </strong></i>
</details>

<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Sort each input array in descending order, then determine which students will be in the front and back rows following Hint #2. After this, simply loop through your sorted input arrays, and check if the current tallest student in the back row is taller than the current tallest student in the front row. If you find that the current tallest student (one that has yet to be placed)in the back row isn't taller than the current tallest student in the front row, then the photo can't be taken. </strong></i>
</details>


<br>

## Method 1

```tex
【O(nlog(n))time∣O(n)space】
```

```java
package Greedy;

import java.util.ArrayList;
import java.util.Collections;

/**
 * @author zhengstars
 * @date 2023/05/05
 */
public class CountSquares {
    public boolean classPhotos(ArrayList<Integer> redShirtHeights, ArrayList<Integer> blueShirtHeights) {
        // Sort the arrays of heights of students with red shirts and blue shirts.
        Collections.sort(redShirtHeights);
        Collections.sort(blueShirtHeights);

        // Calculate the difference between the minimum height of a student with a red shirt
        // and the minimum height of a student with a blue shirt.
        int diff = redShirtHeights.get(0) - blueShirtHeights.get(0);

        // Traverse the arrays of heights of students with red shirts and blue shirts
        // and check the signs of the differences between the corresponding elements of the two arrays.
        for(int i = 0; i < redShirtHeights.size(); i++) {
            if((redShirtHeights.get(i) - blueShirtHeights.get(i)) * diff <= 0){
                // If there is a pair of corresponding elements whose differences have different signs, return false.
                return false;
            }
        }
        // If all corresponding elements have differences with the same sign, return true.
        return true;
    }
}
```

## Method 2

```tex
【O(nlog(n))time∣O(n)space】
```

```java
package Greedy;

import java.util.ArrayList;

/**
 * @author zhengstars
 * @date 2023/05/05
 */
public class classPhotos {
    public static boolean classPhotos(ArrayList<Integer> redShirtHeights, ArrayList<Integer> blueShirtHeights) {
        // Convert input arrays to primitive arrays and sort them
        int[] redShirtHeightsArr = redShirtHeights.stream().mapToInt(i -> i).sorted().toArray();
        int[] blueShirtHeightsArr = blueShirtHeights.stream().mapToInt(i -> i).sorted().toArray();

        // Check the color of the first student's shirt, if it's red, students wearing red shirts should be in the back row,
        // otherwise they should be in the front row
        boolean redInBackRow = redShirtHeightsArr[0] > blueShirtHeightsArr[0];

        // Check each pair of students according to the rules,
        // if a pair does not meet the rules, then a group photo cannot be taken
        for (int i = 0; i < redShirtHeightsArr.length; i++) {
            int redShirtHeight = redShirtHeightsArr[i];
            int blueShirtHeight = blueShirtHeightsArr[i];

            if (redInBackRow) {
                if (redShirtHeight <= blueShirtHeight) {
                    return false;
                }
            } else {
                if (blueShirtHeight <= redShirtHeight) {
                    return false;
                }
            }
        }

        // If all students meet the rules, then a group photo can be taken
        return true;
    }
}
```

