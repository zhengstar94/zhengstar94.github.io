---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Find Three Largest Numbers"
date: "2023-05-08"
categories:
  - "Algorithm Searching"
---

# Find Three Largest Numbers [Easy]

- Write a function that takes in an array of at least three integers and, without sorting the input array, returns a sorted array of the three largest integers in the input array.
- The function should return duplicate integers if necessary; for example, it should return [10, 10, 12] for an input array of [10, 5, 9, 10, 12].

**Sample Input **

```
array = [141, 1, 17, -7, -17, -27, 18, 541, 8, 7, 7]
```

**Sample Output **

```
[18, 141, 541]
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Can you keep track of the three largest numbers in an array as you traverse the input array? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Following the suggestion in Hint #1, try traversing the input array and updating the three largest numbers if necessary by shifting them accordingly. </strong></i>
</details>






<br>



## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Searching;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/06/07
 */
public class FindThreeLargestNumbers {
    public static int[] findThreeLargestNumbers(int[] array) {
        // Create an array to store the three largest numbers
        int[] largestNums = {Integer.MIN_VALUE, Integer.MIN_VALUE, Integer.MIN_VALUE};

        // Iterate over the input array
        for (int i = 0; i < array.length; i++) {
            // Compare the current element with the largest numbers

            // If the current element is greater than the third largest number,
            // shift the values and update the third largest number
            if (array[i] > largestNums[2]) {
                largestNums[0] = largestNums[1];
                largestNums[1] = largestNums[2];
                largestNums[2] = array[i];
            }
            // If the current element is greater than the second largest number,
            // shift the values and update the second largest number
            else if (array[i] > largestNums[1]) {
                largestNums[0] = largestNums[1];
                largestNums[1] = array[i];
            }
            // If the current element is greater than the first largest number,
            // update the first largest number
            else if (array[i] > largestNums[0]) {
                largestNums[0] = array[i];
            }
        }

        // Return the array containing the three largest numbers
        return largestNums;
    }


    public static void main(String[] args) {
        int[] array = {141, 1, 17, -7, -17, -27, 18, 541, 8, 7, 7};
        int[] result = findThreeLargestNumbers(array);
        System.out.println(Arrays.toString(result));
    }
}

```

