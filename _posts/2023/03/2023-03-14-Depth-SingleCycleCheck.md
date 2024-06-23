---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Single Cycle Check"
date: "2023-03-14"
categories:
  - "Algorithm Graphs"
---

# Single Cycle Check [Medium]

- You're given an array of integers where each integer represents a jump of its value in the array. For instance, the integer `2` represents a jump of two indices forward in the array; the integer `-3` represents a jump of three indices backward in the array.

- If a jump spills past the array's bounds, it wraps over to the other side. For instance, a jump of
  `-1` at index `0` brings us to the last index in the array. Similarly, a jump of `1` at the last index in the array brings us to index `0`.

- Write a function that returns a boolean representing whether the jumps in the array form a single cycle. A single cycle occurs if, starting at any index in the array and following the jumps, every element in the array is visited exactly once before landing back on the starting index.





**Sample Input **

````
array=[2,3,1,-4,-4,2]
````

**Sample Output **

```
true
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> In order to check if the input array has a single cycle, you have to jump through all of the elements in the array. Could you keep a counter,jump through elements in the array, and stop once you've jumped through as many elements as are contained in the array? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Assume the input array has length n. If you start at index 0 and jump through n
elements, what are the simplest conditions that you can check to see if the array doesn't have a single cycle? </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Given Hint #2, there are 2 conditions that need to be met for the input array to have a single cycle. First, the starting element (in this case,the element at index 0)cannot be jumped through more than once, at the very beginning, so long as you haven't jumped through all of the other elements in the array. Second, the (n+1)th element you jump through, where n is the length of the array, must be the first element you visited: the element at index 0 in this case. </strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Graphs;

/**
 * @author zhengstars
 * @date 2023/03/05
 */
public class SingleCycleCheck {

    public static boolean hasSingleCycle(int[] array) {
        // Record the number of elements that have been traversed
        int numElementsVisited = 0;
        // The element subscript that is currently traversed
        int currentIdx = 0;

        // Continue traversing when there are still elements untraversed
        while (numElementsVisited < array.length) {
            // If you have traversed some elements and returned to the starting point, it does not constitute a single loop.
            if (numElementsVisited > 0 && currentIdx == 0) { 
                return false;
            }

            // Add 1 to the number of elements that have been traversed
            numElementsVisited++;

            // Get the next element subscript that needs to be traversed
            currentIdx = getNextIdx(currentIdx, array); 
        }

        // if you finally return to the starting point, it forms a single cycle, otherwise it does not constitute
        return currentIdx == 0; 
    }

    /**
     * Get the next element subscript that needs to be traversed
     * @param currentIdx
     * @param array
     * @return
     */
    public static int getNextIdx(int currentIdx, int[] array) {
        // Gets the number of steps that the current element needs to jump
        int jump = array[currentIdx];
        // Calculate the subscript of the next element after the jump
        int nextIdx = (currentIdx + jump) % array.length;
        // Deal with subscript out of bounds
        return nextIdx >= 0 ? nextIdx : nextIdx + array.length; 
    }
}

```
