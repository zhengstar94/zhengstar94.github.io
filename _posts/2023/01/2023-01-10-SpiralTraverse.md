---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Spiral Traverse"
date: "2023-01-10"
categories:
  - "Algorithm Array"
---

# Spiral Traverse [Medium]

- Write a function that takes in an n x m two-dimensional array (that can be square-shaped when n =m)and returns a one-dimensional array of all the array's elements in spiral order.
- Spiral order starts at the top left corner of the two-dimensional array,goes to the right, and proceeds in a spiral pattern all the way until every element has been visited.

**Sample Input**

> array =[
> 	[1,    2,   3,  4],
> 	[12, 13, 14, 5],
> 	[11, 16, 15, 6],
> 	[10,   9,   8, 7],
> ]

**Sample Output**

> [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> You can think of the spiral that you have to traverse as a set of rectangle perimeters that progressively get smaller (i.e.,that progressively move inwards in the two-dimensional array). </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Going off of Hint #1,declare four variables:a starting row,a starting column,an ending row, and an ending column.These four variables represent the bounds of the first rectangle perimeter in the spiral that you have to traverse.Traverse that perimeter using those bounds, and then move the bounds inwards.End your algorithm once the starting row passes the ending row or the starting column passes the ending column.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> You can solve this problem both iteratively and recursively following very similar logic.  </strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time\ N\ is\ the\ number\ of\ elements\ in\ the\ array\ ∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static List<Integer> spiralTraverse(int[][] array) {
    // Write your code here.

    List<Integer> result = new ArrayList<Integer>();
    
    if(array.length == 0){
      return result;
    }

    int rowBegin = 0;
    int rowEnd = array.length - 1;

    int colBegin = 0;
    int colEnd = array[0].length - 1;

    while(rowBegin <=  rowEnd && colBegin <= colEnd){
      
      //Traverse Left to Right(Top)
      for(int i = colBegin; i <= colEnd; i++){
        result.add(array[rowBegin][i]);
      }
      rowBegin++;
      
      //Traverse Top to Bottom(Right)
      for(int i = rowBegin; i <= rowEnd; i++){
        result.add(array[i][colEnd]);
      }
      colEnd--;

      //this is prevent boundary in advance
      if(rowBegin > rowEnd || colBegin > colEnd){
        break;
      }
      
      //Traverse Right to Left(Bottom)
      for(int i = colEnd; i >= colBegin; i--){
        result.add(array[rowEnd][i]);
      }
      rowEnd--;

      //Traverse Bottom to Top(Left)
      for(int i = rowEnd; i >= rowBegin; i--){
        result.add(array[i][colBegin]);
      }
      colBegin++;
      
    }
    return result;
  }
}
```



## Method 2

```tex
【O(n)time\ N\ is\ the\ number\ of\ elements\ in\ the\ array\ ∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static List<Integer> spiralTraverse(int[][] array) {
    List<Integer> result = new ArrayList<Integer>();
    
    spiralFill(array, 0, array.length - 1, 0, array[0].length - 1, result);
      
    return result;
  }

  public static void spiralFill(int[][] array, int rowBegin, int rowEnd, int colBegin, int colEnd, List<Integer> result){
        if(rowBegin > rowEnd || colBegin > colEnd){
            return;
        }

        //Traverse Left to Right(Top)
        for(int i = colBegin; i <= colEnd; i++){
            result.add(array[rowBegin][i]);
        }

        //Traverse Top to Bottom(Right)
        // Note: next time traverse rowBegin need plus 1
        for(int i = rowBegin + 1; i <= rowEnd; i++){
            result.add(array[i][colEnd]);
        }

        //Return if we've processed the last row/column because we'd otherwise repeat values
        if(rowBegin == rowEnd || colBegin == colEnd){
            return;
        }

        //Traverse Right to Left(Bottom)
        // Note: next time traverse colEnd need minus 1
        for(int i = colEnd - 1; i >= colBegin; i--){
            result.add(array[rowEnd][i]);
        }

        //Traverse Bottom to Top(Left)
        // Note: next time traverse rowEnd need minus 1, and rowBegin's boundary need plus 1
        for(int i = rowEnd - 1; i >= rowBegin + 1; i--){
            result.add(array[i][colBegin]);
        }


      spiralFill(array, rowBegin + 1, rowEnd - 1, colBegin + 1, colEnd - 1, result);
  }
}

```

