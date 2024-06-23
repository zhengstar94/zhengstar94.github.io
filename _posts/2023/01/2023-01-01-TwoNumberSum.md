---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Two Number Sum-Easy"
date: "2023-01-01"
categories:
  - "Algorithm Array"
---

# TwoNumberSum[Easy]

- Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum.
  If any two numbers in the input array sum up to the target sum,the function should return them in an array,in any order.
  If no two numbers sum up to the target sum,the function should return an empty array.

- Note that the target sum has to be obtained by summing two different integers in the array;you can't add a single integer to itself in order to obtain the target sum.

- You can assume that there will be at most one pair of numbers summing up to the target sum.

**Sample Input**
> array=[3,5,-4,8,11,1,-1,6] <br>
> targetSum=10

**Sample Output**
> [-1,11] /the numbers could be in reverse order


**Hints**

{% details Hint 1 %} Try using two for loops to sum all possible pairs of numbers in the input array.What are the time and space implications of this approach?  {% enddetails %}

{% details Hint 2 %} Realize that for every number X in the input array,you are essentially trying to find a corresponding number Y such that X Y targetSum.With two variables in this equation known to you,it shouldn't be hard to solve for Y.  {% enddetails %}

{% details Hint 3 %} Try storing every number in a hash table,solving the equation mentioned in Hint #2 for every number,and checking if the Y that you find is stored in the hash table.What are the time and space implications of this approach? {% enddetails %}




## Method 1

```tex 
【 O(n^2)time | O(1)space 】
```



```java
import java.util.*;

class Program {
    public static int[] twoNumberSum(int[] array, int targetSum) {
        for(int i = 0; i <= array.length - 1; i++){
            for(int j = i + 1; j <= array.length - 1; j++){
                if(array[i]+array[j]==targetSum){
                    return new int[]{array[i],array[j]};
                }
            }
        }
        return new int[0];
    }
}
```



## Method 2

```tex 
【 O(n)time | O(n)space 】
```

```java
import java.util.*;

class Program {
  public static int[] twoNumberSum(int[] array, int targetSum) {
    // Write your code here.
    List<Integer> temp = new ArrayList<Integer>();
    for(int i = 0; i < array.length; i++){
      if(temp.contains(targetSum - array[i])){
         return new int[]{targetSum - array[i],array[i]};
      }
      temp.add(array[i]);
    }
    return new int[0];
  }
}
```

## Method 3

```tex 
【 O(nlog(n))time | O(1)space 】
```

```java
import java.util.*;

class Program {
    public static int[] twoNumberSum(int[] array, int targetSum) {
        Arrays.sort(array);
        int left = 0;
        int right = array.length - 1;
        while(left < right){
            if(array[left] + array[right] == targetSum){
                return new int[]{array[left],array[right]};
            }else if (array[left] + array[right] < targetSum){
                left ++;
            }else{
                right --;
            }
        }
        return new int[0];
    }
}
```





