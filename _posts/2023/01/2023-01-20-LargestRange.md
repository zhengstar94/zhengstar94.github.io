---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Largest Range"
date: "2023-01-20"
categories:
  - "Algorithm Array"
---


# Largest Range [Hard]

- Write a function that takes in an array of integers and returns an array of length 2 representing the largest range of integers contained in that array.
- The first number in the output array should be the first number in the range, while the second number should be the last number in the range.
- A range of numbers is defined as a set of numbers that come right after each other in the set of real integers. For instance, the output array `[2,6]`represents the range`{2,3,4,5,6}`, which is a range of length 5. Note that numbers don't need to be sorted or adjacent in the input array in order to form a range.
- You can assume that there will only be one largest range.

**Sample Input #1**

> array=[1,11,3,0,15,5,2,4,10,7,12,6]

**Sample Output #1**

> [0,7]

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> How can you use a hash table to solve this problem with an algorithm that runs in linear time?</strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Iterate through the input array once,storing every unique number in a hash table and mapping every number to a falsy value.This hash table will not only provide for fast access of the numbers in the input array,but it will also allow you to keep track of "visited"and "unvisited"numbers,so as not to unnecessarily repeat work.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Iterate through the input array once more,this time stopping at every number to check if the number is marked as "visited"in the hash table.If it is,skip it;if it isn't,start expanding outwards from that number with a left number and a right number,continuously checking if those left and right numbers are in the hash table (and thus in the input array),and marking them as "visited"in the hash table if they are.This should allow you to quickly find the largest range in which the current number is contained,all the while setting you up not to perform unnecessary work later.  </strong></i>
</details>

<br>


## Method 1

```tex
【 O(nlog(n))time | O(1) space 】
```



```java
import java.util.*;

class Program {
  public static int[] largestRange(int[] array) {
    // Write your code here.
    if(array.length == 1){
      return new int[]{array[0],array[0]};
    }
    Arrays.sort(array); 
    int[] ans = new int[2]; 
    int i=0;
    while(i < array.length - 1){
        int j=i;
        //array[i] == array[i+1] || array[i] + 1 == array[i+1]
        //array[i] == array[i+1] is equal or array[i] + 1 == array[i+1] is equal, it means continuous.
        while(i < array.length - 1 && (array[i] == array[i+1] || array[i] + 1 == array[i+1])){ 
            i++;
        }

        if(array[i] - array[j] > ans[1] - ans[0]){ 
            ans[0] = array[j];
            ans[1] = array[i];
        }
        i++;
    }
    return ans;
  }
}

```



## Method 2 【To be continued】

```tex
【 O(n)time | O(n) space 】
```


```java
import java.util.*;

class Program {
    public static int[] largestRange(int[] array) {
        // Write your code here.

        //Declare a Map collection to store all elements
        Map<Integer, Boolean> map = new HashMap<>(array.length);
        int[] longestRange = new int[]{array[0], array[0]};
        int longestLength = 1;

        //Elements are all true by default
        for (int item : array) {
            map.put(item, true);
        }
        for (int i = 0; i < array.length; i ++) {
            int current = array[i];

            //if map.get(current) = false, then current has been sorted, then move to next number
            if (!map.get(current)) {
                continue;
            }
            int currentLength = 1;

            //Move left from the current 
            int left = current - 1;
            while (map.containsKey(left)) {
                map.put(left, false);
                currentLength ++;
                left --;
            }

            //Move right from the current 
            int right = current + 1;
            while (map.containsKey(right)) {
                map.put(right, false);
                currentLength ++;
                right ++;
            }


            if (currentLength > longestLength) {
                longestLength = currentLength;

                //The last while cycle， left and right value have been modified
                longestRange[0] = left + 1;
                longestRange[1] = right - 1;
            }
        }
        return longestRange;
    }
}


```

