---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Smallest Difference"
date: "2023-01-07"
categories:
  - "Algorithm Array"
---

# Smallest Difference[Medium]

- Write a function that takes in two non-empty arrays of integers,finds the pair of numbers (one from each array)whose absolute difference is closest to zero, and returns an array containing these two numbers, with the number from the first array in the first position.

- Note that the absolute difference of two integers is the distance between them on the real number line. For example, the absolute difference of -5 and 5 is 10, and the absolute difference of -5 and -4 is 1.
- You can assume that there will only be one pair of numbers with the smallest difference.

**Sample Input**

> arrayOne = [-1, 5, 10, 20, 28, 3]<br>arrayTwo = [26, 134, 135, 15, 17]

**Sample Output**

> [28,26]


**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Instead of generating all possible pairs of numbers,try somehow only looking at pairs that you know could actually have the smallest difference.How can you accomplish this? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Would it help if the two arrays were sorted?If the arrays were sorted and you were looking at a given pair of numbers,could you efficiently find the next pair of numbers to look at?What are the runtime implications of sorting the arrays?  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Start by sorting both arrays,as per Hint #2.Put a pointer at the beginning of both arrays and evaluate the absolute difference of the pointer-numbers.If the difference is equal to zero, then you've found the closest pair;otherwise,increment the pointer of the smaller of the two numbers to find a potentially better pair.Continue until you get a pair with a difference of zero or until one of the pointers gets out of range of its array.  </strong></i>
</details>

<br>



## Methon 1



```tex
【O(nm)time∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static int[] smallestDifference(int[] arrayOne, int[] arrayTwo) {
    // Write your code here.
    //Arrays.sort(arrayOne);
    //Arrays.sort(arrayTwo);
    int[] nums = new int[2];
    
    int temp = Integer.MAX_VALUE;
    for(int i = 0; i <  arrayOne.length; i++){
      for(int j = 0; j <  arrayTwo.length; j++){
          int sum =  Math.abs(arrayOne[i] - arrayTwo[j]);
          if(temp >= sum){
             temp = sum;
             nums = new int[]{arrayOne[i],arrayTwo[j]};
          }
      }
   }
    return nums;
  }
}

```



## Methon 2



```tex
【O(nlog(n)+mlog(n)time∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static int[] smallestDifference(int[] arrayOne, int[] arrayTwo) {
    Arrays.sort(arrayOne);
    Arrays.sort(arrayTwo);

    int[] nums = new int[2];

    int temp = Integer.MAX_VALUE;
    int one = 0;
    int two = 0;

    while(one < arrayOne.length && two < arrayTwo.length){
      int sum =  Math.abs(arrayOne[one] - arrayTwo[two]);

      if(temp >= sum){
        temp = sum;
        nums = new int[]{arrayOne[one],arrayTwo[two]};   
      }

      if(arrayOne[one] < arrayTwo[two]){
        one++;
      }else{
        two++;
      }
      
    }
    return nums;
  }
}
```



