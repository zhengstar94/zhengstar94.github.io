---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Longest Peak"
date: "2023-01-11"
categories:
  - "Algorithm Array"
---

# Longest Peak [Medium]

- Write a function that takes in an array of integers and returns the length of the longest peak in the array.
- A peak is defined as adjacent integers in the array that are strictly increasing until they reach a tip (the highest value in the peak), at which point they become strictly decreasing. At least three integers are required to form a peak.
- For example,the integers `1,4,10,2` form a peak,but the integers `4,0,10` don't and neither do the integers `1,2,2,0`.  Similarly, the integers `1,2,3` don't form a peak because there aren't any strictly decreasing integers after the `3`.

**Sample Input**

> array=[1,2,3,3,4,0,10,6,5,-1,-3,2,3]

**Sample Output**

> 6//0,10,6,5,-1,-3

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> You can solve this question by iterating through the array from left to right once. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Iterate through the array from left to right,and treat every integer as the potential tip of a peak.To be the tip of a peak,an integer has to be strictly greater than its adjacent integers. What can you do when you find an actual tip?  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> As you iterate through the array from left to right,whenever you find a tip of a peak,expand outwards from the tip until you no longer have a peak.Given what peaks look like and how many peaks can therefore fit in an array,realize that this process results in a linear-time algorithm.Make sure to keep track of the longest peak you find as you iterate through the array. </strong></i>
</details>

<br>

## Method 1



```tex
【O(n)time\ N\ is\ the\ length\ of\ the\ array\ ∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static int longestPeak(int[] array) {
    // Write your code here.
    int longestNumber = 0;

    int arrayLength = array.length - 1;
    
    if(array.length < 3){
      return longestNumber;
    }

    for(int i = 1; i < arrayLength; i++){
      if(array[i] > array[i - 1] && array[i] > array[i + 1]){
          int left = i - 1;
          int right = i + 1;

          //left boundary
          while(left > 0 && array[left] > array[left-1]){
            left--;
          }

          //right boundary
          while(right < arrayLength && array[right] > array[right+1]){
            right++;
          }

          longestNumber = Math.max(longestNumber, right-left+1);
        
      }
    }
     
    return longestNumber;
  }
}

```

