---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Missing Numbers"
date: "2023-01-16"
categories:
  - "Algorithm Array"
---

# Missing Numbers [Medium]

- You're given an unordered list of unique integers `nums` in the range `[1,n]`, where `n` represents the length of `nums+2`. This means that two numbers in this range are missing from the list.
- Write a function that takes in this list and returns a new list with the two missing numbers, sorted numerically.

**Sample Input**

```
nums=[1,4,3]
```

**Sample Output**

```
[2,5] //n is 5,meaning the completed list should be [1,2,3,4,5]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> How would you solve this problem if there was only one missing number? Can that solution be applied to this problem with two missing numbers? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> To efficiently find a single missing number,you can sum up all of the values in the array as well as sum up all of the values in the expected array (i.e. in the range [1,n]). The difference between these values is the missing number. </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Using the same logic as for a single missing number, you can find the total of the two missing numbers. How can you then find which numbers these are? </strong></i>
</details>


<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> If you take an average of the two missing numbers,one of the missing numbers must be less than that average, and one must be greater than the average. </strong></i>
</details>


<br>

<details> <summary><b>Hint 5</b></summary>
    <br>
    <i><strong> Since we know there is one missing number on each side of the average, we can treat each side of the list as its own problem to find one missing number in that list. </strong></i>
</details>


<br>

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Array;

/**
 * @author zhengstars
 * @date 2023/04/17
 */
public class MissingNumbers {
    public static int[] findMissingNumbers(int[] nums) {
        // The length of the complete array
        int n = nums.length + 2;
        // The sum of numbers in the complete array
        long sum = (long)n * (n + 1) / 2;
        // The sum of numbers in the given array
        long arrSum = 0;
        for (int num : nums) {
            arrSum += num;
        }

        // The sum of the two missing numbers
        long pivot = (sum - arrSum) / 2;
        // The sum of numbers in the left half of the complete array
        long leftSum = 0;
        // The sum of numbers in the left half of the given array
        long arrLeftSum = 0;

        // Traverse the numbers in the range [1, pivot]
        for (int i = 1; i <= pivot; i++) {
            leftSum += i;
        }

        // Calculate the sum of the given numbers in the range [1, pivot]
        for (int num : nums) {
            if (num <= pivot) {
                arrLeftSum += num;
            }
        }

        // Calculate the left missing number
        long missingLeft = leftSum - arrLeftSum;
        // Calculate the right missing number
        long missingRight = sum - missingLeft - arrSum;
        // Combine the missing numbers
        int[] result = {(int)missingLeft, (int)missingRight};
        return result;
    }
}
```

