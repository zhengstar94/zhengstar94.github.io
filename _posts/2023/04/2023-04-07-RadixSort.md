---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Radix Sort"
date: "2023-04-07"
categories:
  - "Algorithm Sorting"
---

# Radix Sort [Hard]

- Write a function that takes in an array of non-negative integers and returns a sorted version of that array. Use the Radix Sort algorithm to sort the array.
- If you're unfamiliar with Radix Sort we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

**Sample Input**

```
array=[8762,654,3008,345,87,65,234,12,2]
```

**Sample Output**

```
[2,12,65,87,234,345,654,3008,8762]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Radix Sort sorts numbers by looking only at one of their digits at a time. It first sorts all of the given numbers by their ones'column, then by their tens'column, then by their hundreds column, and so on and so forth until they're fully sorted. </strong></i>
</details>


<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Radix Sort uses an intermediary sorting algorithm to sort numbers one digits'column at a time. The goal of Radix Sort is to perform a more efficient sort than popular sorting algorithms like Merge Sort or Quick Sort for inputs that are well suited to be sorted by their individual digits columns. With this in mind, what intermediary sorting algorithm should we use with Radix Sort? Keep in mind that this sorting algorithm will run multiple times,sorting one digits'column at a time. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> The most popular sorting algorithm to use with Radix Sort is Counting Sort.Counting Sort takes advantage of the fact that we know the range of possible values that we need to sort. When sorting numbers,we know that we only need to sort digits, which will always be in the range 0-9 Therefore, we can count how many times these digits occur and use those counts to populate a new sorted array. We'll perform counting sort multiple times,once for each digits' column that we're sorting, starting with the ones'column. We need to ensure that our counting sort performs a stable sort, so that we don't lose information from previous iterations of sorting. Counting sort runs in 0(n) time, which means that we might have a much more efficient sorting algorithm if the largest number in our input contains few digits. See the Conceptual Overview section of this question's video explanation for a more in-depth explanation. </strong></i>
</details>



<br>

## Method 1

```tex
【O(d * (n+k))time∣O(n+k)space】
```
<br>


```tex

1.\ D\ is\ number\ of\ digits\\
2.\ N\ is\ the\ number\ of\ digits\ to\ be\ sorted\\ 
3.\ K\ is\ the\ number\ of\ different\ numbers\ that\ may\ appear\ in\ each\ number.
```

```java
package Sorting;

import java.util.ArrayList;
import java.util.Collections;

/**
 * @author zhengstars
 * @date 2023/04/13
 */
public class RadixSort2 {
    public static ArrayList<Integer> radixSort(ArrayList<Integer> array) {
        // If the array is empty, return it directly
        if (array.size() == 0) {
            return array;
        }
        // Find the maximum number in the array
        int maxNum = Collections.max(array);
        // Sort from the unit digit
        int digit = 0;
        // Continue sorting while the maximum number divided by 10 to the power of digit is greater than 0
        while (maxNum / Math.pow(10, digit) > 0) {
            // Sort the array according to the digit bit
            countingSort(array, digit);
            // Sort the next bit
            digit++;
        }
        // Return the sorted array
        return array;
    }

    public static void countingSort(ArrayList<Integer> array, int digit) {
        // Array used for counting
        int[] countArray = new int[10];
        // The radix of the current bit
        int digitColumn = (int) Math.pow(10, digit);

        // Traverse the array
        for (int num : array) {
            // Find the digit at the current bit
            int countIndex = (num / digitColumn) % 10;
            // Increase the counter for the corresponding digit by 1
            countArray[countIndex]++;
        }

        // Calculate the prefix sum
        for (int i = 1; i < 10; i++) {
            countArray[i] += countArray[i - 1];
        }

        // Array used for storing the sorted array
        int[] tempArray = new int[array.size()];
        // Traverse the array from back to front
        for (int i = array.size() - 1; i >= 0; i--) {
            // Find the digit at the current bit
            int countIndex = (array.get(i) / digitColumn) % 10;
            // Put the number in the corresponding position
            tempArray[countArray[countIndex] - 1] = array.get(i);
            // Decrease the counter for the corresponding digit by 1
            countArray[countIndex]--;
        }

        // Copy the sorted array back to the original array
        for (int i = 0; i < array.size(); i++) {
            array.set(i, tempArray[i]);
        }
    }
}

```

