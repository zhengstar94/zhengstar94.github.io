---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "First Duplicate Value"
date: "2023-01-13"
categories:
  - "Algorithm Array"
---

# First Duplicate Value [Medium]

- Given an array of integers between `1` and `n`,  inclusive, where `n` is the length of the array,  write a function that returns the first integer that appears more than once (when the array is read from left to right).
- In other words, out of all the integers that might occur more than once in the input array, your function should return the one whose first duplicate value has the minimum index.
- If no integer appears more than once,your function should return `-1`.
- Note that you're allowed to mutate the input array.

**Sample Input #1**

> array=[2,1,5,2,3,3,4]

**Sample Output #1**

> 2 // 2 is the first integer that appears more than once.
>
> // 3 also appears more than once, but the second 3 appears after the second 2.



**Sample Input #2**

> array=[2,1,5,3,3,2,4]

**Sample Output #2**

> 3 // 3 is the first integer that appears more than once.
>
> // 2 also appears more than once, but the second 2 appears after the second 3.


**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> The brute-force solution can be done in O(n^2)time.Think about how you can determine if a value appears twice in an array. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> You can use a data structure that has constant-time lookups to keep track of integers that you've seen already.This leads the way to a linear-time solution.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> You should always pay close attention to the details of a question's prompt.In this question, the integers in the array are between 1 and n,inclusive,where n is the length of the input array.The prompt also explicitly allows us to mutate the array.How can these details help us find a better solution,either time-complexity-wise or space-complexity-wise? </strong></i>
</details>

<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Since the integers are between 1 and the length of the input array,you can map them to indices in the array itself by subtracting 1 from them.Once you've mapped an integer to an index in the array,you can mutate the value in the array at that index and make it negative (by multiplying it by -1).Since the integers normally aren't negative,the first time that you encounter a negative value at the index that an integer maps to,you'll know that you'll have already seen that integer.  </strong></i>
</details>

<br>


## Method 1

```tex
【 O(n) time | O(n) space 】
```



```java
import java.util.*;

class Program {

  public int firstDuplicateValue(int[] array) {

   Set<Integer> set = new HashSet<Integer>();
   for(int i = 0; i < array.length ; i++){
      if(set.contains(array[i])){
        return array[i];
      }else{
        set.add(array[i]);
      }
   }
    return -1;
  }
}
```



## Method 2


```tex
【 O(n) time | O(1) space 】
```



```java
import java.util.*;

class Program {

  public int firstDuplicateValue(int[] array) {
    // loop through the array of integers
    for(int i = 0; i < array.length; i++){
      // calculate the new index based on the absolute value of the current integer in the array  
      int newIndex = Math.abs(array[i]) - 1;
      // if the value at the new index is negative, return the absolute value of the current integer
      if(array[newIndex] < 0){
        return Math.abs(array[i]);
      }
      // otherwise, negate the value at the new index to mark it as visited
      array[newIndex] = array[newIndex] * -1;
    }
    // if no duplicates are found, return -1
    return -1;
  }
}
```

