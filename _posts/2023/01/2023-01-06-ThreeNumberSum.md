---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Three Number Sum"
date: "2023-01-06"
categories:
  - "Algorithm Array"
---

# Three Number Sum[Medium]

- Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all triplets in the array that sum up to the target sum and return a two-dimensional array of all these triplets. The numbers in each triplet should be ordered in ascending order, and the triplets themselves should be ordered in ascending order with respect to the numbers they hold.

- lf no three numbers sum up to the target sum, the function should return an empty array.

**Sample Input**

> array = [12,3,1,2,-6,5,-8,6]<br>targetSum = 0

**Sample Output**

> [[-8,2,6], [-8,3,5], [-6,1,5]]


**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Using three for loops to calculate the sums of all possible triplets in the array would generate an algorithm that runs in O(n^3)time,where n is the length of the input array.Can you come up with something faster using only two for loops? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Try sorting the array and traversing it once.At each number,place a left pointer on the number immediately to the right of your current number and a right pointer on the final number in the array.Check if the current number,the left number,and the right number sum up to the target sum.How can you proceed from there,remembering the fact that you sorted the array?  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Since the array is now sorted (see Hint #2),you know that moving the left pointer mentioned in Hint #2 one place to the right will lead to a greater left number and thus a greater sum.
Similarly,you know that moving the right pointer one place to the left will lead to a smaller right number and thus a smaller sum.This means that,depending on the size of each triplet's (current number,left number,right number)sum relative to the target sum,you should either move the left pointer,the right pointer,or both to obtain a potentially valid triplet.  </strong></i>
</details>

<br>




## Method1

```tex
【 O(n^3)time | O(n) space 】
```



```java
import java.util.*;

class Program {
  public static List<Integer[]> threeNumberSum(int[] array, int targetSum) {
    // Write your code here.
    Arrays.sort(array);
    
    List<Integer[]> list = new ArrayList<Integer[]>();

    for(int i = 0; i < array.length; i++){
      for(int j = i+1; j < array.length; j++){
        for(int k = j+1; k < array.length; k++){
          if(array[i]+array[j]+array[k] == targetSum){
            list.add(new Integer[]{array[i],array[j],array[k]});
          }
        }
      }   
    }
    return list;
  }
}
```



## Method2

```tex
【 O(n^2)time | O(n) space 】\\
 \\
```

<br>

```tex
1.\ Array\ sort\ O(nlogn)\\
2.\ Traversal\ Array\ O(n)\ *\ Double\ pointer\ Traversal\ O(n)
```


```java
import java.util.*;

class Program {
  public static List<Integer[]> threeNumberSum(int[] array, int targetSum) {
    // Write your code here.
    Arrays.sort(array);
    
    List<Integer[]> list = new ArrayList<Integer[]>();


    for(int i = 0; i < array.length; i++){
      int sum = targetSum - array[i];
      
      int left = i+1;
      int right = array.length-1;

      while(left<right){
        if(array[left] + array[right] == sum){
          list.add(new Integer[]{array[i],array[left],array[right]});
          left++;
          right--;
        }else if(array[left] + array[right] < sum){
          left++;
        }else{
          right--;
        }
      }
    } 
    return list;
  }
}
```

