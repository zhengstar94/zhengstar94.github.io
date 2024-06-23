---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Zigzag Traverse"
date: "2023-01-22"
categories:
  - "Algorithm Array"
---

# Zigzag Traverse [Hard]

- Write a function that takes in an n x m two-dimensional array (that can be square-shaped when n =m)and returns a one-dimensional array of all the array's elements in zigzag order.
- Zigzag order starts at the top left corner of the two-dimensional array, goes down by one element, and proceeds in a zigzag pattern all the way to the bottom right corner.

**Sample Input #1**

> array = [
> [1,3,4,10],
> [2,5,9,11],
> [6,8,12,15],
> [7,13,14,16],
> ]

**Sample Output #1**

> [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Don't overthink this question by trying to come up with a clever way of getting the zigzag order.Think about the simplest checks that need to be made to decide when and how to change direction throughout the zigzag traversal. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Starting at the top left corner,iterate through the two-dimensional array by keeping track of the direction that you're moving in (up or down).If you're moving up,you know that you need to move in an up-right pattern and that you need to handle the case where you hit the top or the right borders of the array.If you're moving down,you know that you need to move in a down-left pattern and that you need to handle the case where you hit the left or the bottom borders of the array.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> When going up,if you hit the right border,you'll have to go down one element;if you hit the top border,you'll have to go right one element.Similarly,when going down,if you hit the left border,you'll have to go down one element;if you hit the bottom border,you'll have to go right one element.  </strong></i>
</details>

<br>

## Method 1



```tex
【O(n)time∣O(n)space】
```



```java
import java.util.*;

class Program {

    public static List<Integer> zigzagTraverse(List<List<Integer>> array) {
        // Write your code here.
        List<Integer> result = new ArrayList<Integer>();
        int height = array.size() - 1;
        int width = array.get(0).size() - 1;
        int row = 0;
        int col = 0;
        //whether goingDown,default true
        boolean goingDown = true;

        while(!isOutofBound(row,col,height,width)){
            result.add(array.get(row).get(col));
            // if goingDown
            if(goingDown){
                //if target is the boundary value（left and bottom）
                if(col == 0 || row == height){

                    //the next step need goingUp
                    goingDown = false;


                    if(row == height){
                        //bottom boundary，for example 13->14,
                        col +=1;
                    }else{
                        // left boundary，for example 1->2 , 6->7
                        row +=1;
                    }
                }else{
                    //goingDown, Diagonal line 4->5, 5->6, 11->12, 12->13
                    row +=1;
                    col -=1;
                }
            }else{
                // if goingDown

                //if target is the boundary value（right and top）
                if(row == 0 || col == width){

                    //the next step need goingDown
                    goingDown = true;

                    // right boundary
                    if(col == width){
                        //10->11, 15->16
                        row +=1;
                    }else{
                        //top boundary，for example 3->4
                        col +=1;
                    }
                }else{
                    //goingUp, Diagonal line 2->3, 7->8, 8->9, 9->10, 14->15
                    row -=1;
                    col +=1;

                }
            }
        }

        return result;
    }

    //whether to exceed the boundary
    public static boolean isOutofBound(int row,int col,int height,int width){
        return row < 0 ||  row > height || col < 0 ||  col > width;
    }

}

```

