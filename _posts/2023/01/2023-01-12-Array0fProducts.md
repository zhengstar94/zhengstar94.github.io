---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Array Of Products"
date: "2023-01-12"
categories:
  - "Algorithm Array"
---

# Array Of Products [Medium]

- Write a function that takes in a non-empty array of integers and returns an array of the same length, where each element in the output array is equal to the product of every other number in the input array.
- In other words, the value at `output [i]`is equal to the product of every number in the input array other than `input[i]`.
- Note that you're expected to solve this problem without using division.

**Sample Input**

> array=[5,1,4,2]

**Sample Output**

> [8,40,10,20]
>
> // 8 is equal to 1 x 4 x 2
>
> // 40 is equal to 5 x 4 x 2
>
> // 10 is equal to 5 x 1 x 2
>
> // 20 is equal to 5 x 1 x 4


**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Think about the most naive approach to solving this problem.How can we do exactly what the problem wants us to do without focusing at all on time and space complexity? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Understand how output[i]is being calculated.How can we calculate the product of every element other than the one at the current index?Can we do this with just one loop through the input array,or do we have to do multiple loops?  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> For each index in the input array,try calculating the product of every element to the left and the product of every element to the right.You can do this with two loops through the array: one from left to right and one from right to left.How can these products help us? </strong></i>
</details>

<br>

## Method 1



```tex
【 O(n^2) time | O(n) space 】
```



```java
import java.util.*;

class Program {
  public int[] arrayOfProducts(int[] array) {
    // Write your code here.
    
    int[] temp = new int[array.length];
    for(int i = 0; i < array.length; i++){
      int sum = 1;
      for(int j = 0; j < array.length; i++){
          if(i != j){
              sum *= array[j];
          }
      }
      temp[i] = sum;
      
    }
    return temp;
  }
}

```

## Method 2



```tex
【 O(n) time | O(n) space 】
```



```java
import java.util.*;

class Program {
  public int[] arrayOfProducts(int[] array) {
    // Write your code here.
    int[] temp = new int[array.length];

    for(int i = 0; i < array.length; i++ ){
      int leftProduct = 1;
      int rightProduct = 1;
      int leftIdx = 0;
      int rightIdx = array.length - 1;

      while(leftIdx >= 0 && leftIdx < i ){
        leftProduct *= array[leftIdx];
        leftIdx++;
      }
      
      while(rightIdx > i && rightIdx <=  array.length - 1){
        rightProduct *= array[rightIdx];
        rightIdx--;
      }


      temp[i] = (leftProduct*rightProduct);
    }

    
    return temp;
  }
}

```

## Method 3

**[Video commentary](https://www.youtube.com/watch?v=rpQhKorJRd8)**


```tex
【 O(n) time | O(n) space 】
```



```java
import java.util.*;

class Program {
    public int[] arrayOfProducts(int[] nums) {
        int[]res =  new int[nums.length];
        //left to right
        res[0]=1;
        for (int i=1;i<nums.length;i++) {
            res[i] = res[i -1] * nums[i-1];
        }
        //right to left
        int right = 1;
        for (int i=nums.length -1;i>=0;i--){
            res[i] = res[i] * right;
            right = right * nums[i];
        }
        return res;
    }
}

```

