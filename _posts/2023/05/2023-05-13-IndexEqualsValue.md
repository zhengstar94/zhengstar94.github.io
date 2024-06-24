---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Index Equals Value"
date: "2023-05-13"
categories:
  - "Algorithm Searching"
---

# Index Equals Value [Hard]

- Write a function that takes in a sorted array of distinct integers and returns the first index in the array that is equal to the value at that index. In other words, your function should return the minimum index where index == array[index].

- If there is no such index, your function should return -1.



**Sample Input **

```
array = [-5, -3, 0, 3, 4, 5, 9]
```

**Sample Output **

```
3 // 3 == array[3]
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>First think about a simple brute-force approach to solve this problem. What is the time complexity of this approach and what improvements could be made to this time complexity? </strong></i>
</details>







<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>If the brute force solution runs in linear time complexity, then a better solution would have to run in O(log(n)) time. Which algorithm has an O(log(n)) time complexity? </strong></i>
</details>



<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Implement a variation of binary search to solve this problem. Think about what conditions or checks must be added to search for the desired index-value pair. </strong></i>
</details>



<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>As you perform a variation of binary search on the input array, if the value that you're looking at is smaller than its index, cut the left half of the array from the search space, because all values to the left will be smaller than their corresponding indices; this is guaranteed to be true, since left indices will naturally decrement by 1 each and left values will decrement by at least 1 each due to the array being sorted. Similar logic applies to the right side of the array when the value that you're looking at is greater than its index. </strong></i>
</details>


<br>



<details> <summary><b>Hint 5</b></summary>
    <br>
    <i><strong>When you encounter a value that's equal to its index, you'll have to perform some additional logic to make sure that you're not potentially missing other values in the array that are equal to their index and that come before the value that you're looking at. </strong></i>
</details>



<br>



## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Searching;

/**
 * @author zhengstars
 * @date 2023/06/10
 */
public class IndexEqualsValue {
    public static int indexEqualsValue(int[] array) {
        int left = 0;
        int right = array.length - 1;

        while (left <= right) {
            int middle = left + (right - left) / 2;
            int currentValue = array[middle];

            if (middle == currentValue) {
                return middle;
            } else if (middle > currentValue) {
                left = middle + 1;
            } else {
                right = middle - 1;
            }
        }

        return -1;
    }

    public static void main(String[] args) {
        int[] array = {-5, -3, 0, 3, 4, 5, 9};
        int result = indexEqualsValue(array);
        System.out.println(result); 
    }
}
```

