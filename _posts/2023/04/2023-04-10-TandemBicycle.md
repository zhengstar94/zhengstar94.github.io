---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Tandem Bicycle"
date: "2023-04-10"
categories:
  - "Algorithm Greedy"
---

# Tandem Bicycle [Easy]

- A tandem bicycle is a bicycle that's operated by two people: person A and person B. Both people pedal the bicycle, but the person that pedals faster dictates the speed of the bicycle. So if person A pedals at a speed of `5`, and person B pedals at a speed of `4`, the tandem bicycle moves at a speed of `5` (i.e., `tandemSpeed max(speedA,speedB)`).
- You're given two lists of positive integers: one that contains the speeds of riders wearing red shirts and one that contains the speeds of riders wearing blue shirts. Each rider is represented by a single positive integer, which is the speed that they pedal a tandem bicycle at. Both lists have the same length, meaning that there are as many red-shirt riders as there are blue-shirt riders. Your goal is to pair every rider wearing a red shirt with a rider wearing a blue shirt to operate a tandem bicycle.
- Write a function that returns the maximum possible total speed or the minimum possible total speed of all of the tandem bicycles being ridden based on an input parameter, `fastest`. If `fastest=true`, your function should return the maximum possible total speed; otherwise it should return the minimum total speed.
- "Total speed" is defined as the sum of the speeds of all the tandem bicycles being ridden. For example, if there are 4 riders (2 red-shirt riders and 2 blue-shirt riders)who have speeds of `1,3,4,5`, and if they're paired on tandem bicycles as follows: `[1,4],[5,3]`, then the total speed of these tandem bicycles is `4 + 5 = 9`.

**Sample Input**

```
redshirtspeeds = [5,5,3,9,2] 
blueShirtspeeds = [3,6,7,2,1] 
fastest = true
```

**Sample Output**

```
32
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> The brute-force approach to solve this problem is to generate every possible set of pairs of riders and to determine the total speed that each of these sets generates. This solution does work but, it isn't optimal. Can you think of a better way to solve this problem? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Try looking at the input arrays in sorted order. How might this help you solve the problem? </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> When generating the maximum total speed, you want to pair the slowest red-shirt riders with the fastest blue-shirt riders and vice versa, so as to always take advantage of the largest speeds. When generating the minimum total speed, you want to pair the fastest red-shirt riders with the fastest blue-shirt riders, so as to "eliminate" a large speed by pairing it with a another large (larger) speed. </strong></i>
</details>


<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Sort the input arrays in place, and follow the strategy discussed in Hint #3. With the inputs sorted, you can find the slowest and largest speeds from each shirt color in constant time. </strong></i>
</details>


<br>

## Method 1

```tex
【O(nlog(n))time∣O(1)space】
```

```java
package Greedy;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/05/06
 */
public class TandemBicycle2 {
    public int tandemBicycle(int[] redShirtSpeeds, int[] blueShirtSpeeds, boolean fastest) {
        // Sort the two arrays
        Arrays.sort(redShirtSpeeds);
        Arrays.sort(blueShirtSpeeds);

        // Initialize the total speed
        int total = 0;

        // Determine the direction of pointer movement
        int direction =  fastest ? -1 : 1;

        // Set the starting position of the red-shirt pointer based on the input parameter
        int redPointer = fastest ? redShirtSpeeds.length - 1 : 0;

        // The starting position of the blue-shirt pointer is 0
        int bluePointer = 0;

        // Traverse the speed list of blue-shirt riders
        while (bluePointer < blueShirtSpeeds.length) {
            // Choose the faster of the two riders and add their speed to the total speed
            total += Math.max(redShirtSpeeds[redPointer], blueShirtSpeeds[bluePointer++]);
            // Move the pointer one step in the specified direction
            redPointer += direction;
        }

        // Return the total speed
        return total;
    }
}
```

