---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Validate Subsequence"
date: "2023-01-02"
categories:
  - "Algorithm Array"
---

# Validate Subsequence[Easy]

- Given two non-empty arrays of integers,write a function that determines whether the second array is a subsequence of the first one.

- A subsequence of an array is a set of numbers that aren't necessarily adjacent in the array but that are in the same order as they appear in the array. For instance,the numbers `[1,3,4]`form a subsequence of the array `[1,2,3,4]`,and so do the numbers `[2,4]`Note that a single number in an array and the array itself are both valid subsequences of the array.

**Sample Input**
> array=[5,1,22,25,6,-1,8,10] <br>
> sequence=[1,6,-1,10]

**Sample Output**
> true


**Hints**

{% details Hint 1 %} You can solve this question by iterating through the main input array once  {% enddetails %}

{% details Hint 2 %} Iterate through the main array,and look for the first integer in the potential subsequence.If you find that integer,keep on iterating through the main array,but now look for the second integer in the potential subsequence.Continue this process until you either find every integer in the potential subsequence or you reach the end of the main array. {% enddetails %}

{% details Hint 3 %} To actually implement what Hint #2 describes,you'll have to declare a variable holding your position in the potential subsequence.At first,this position will be the Oth index in the sequence;as you find the sequence's integers in the main array,you'll increment the position variable until you reach the end of the sequence. {% enddetails %}



## Method 1

```tex 
【 O(n)time | O(1)space 】
```

```java
import java.util.*;

class Program {
  public static boolean isValidSubsequence(List<Integer> array, List<Integer> sequence) {
    int index = 0;
    for(int i = 0; i < array.size(); i++){
        if(array.get(i).equals(sequence.get(index))){
          index++;
        }
    }

    if(index == sequence.size()){
        return true;
    }
    
    return false; 
  }
}
```

## Method 2

```tex 
【 O(n)time | O(1)space 】
```

```java
import java.util.*;

class Program {
    public static boolean isValidSubsequence(List<Integer> array, List<Integer> sequence) {
        // Write your code here.
        int arrIndex = 0;
        int index = 0;

        while(arrIndex < array.size() && index < sequence.size()){
            if(array.get(arrIndex).equals(sequence.get(index))){
                index++;
            }
            arrIndex++;
        }
        return sequence.size() == index;
    }
}

```
