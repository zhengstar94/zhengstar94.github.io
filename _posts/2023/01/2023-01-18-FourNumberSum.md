---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Four Number Sum"
date: "2023-01-18"
categories:
  - "Algorithm Array"
---

# Four Number Sum [Hard]

- Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all quadruplets in the array that sum up to the target sum and return a two-dimensional array of all these quadruplets in no particular order.
- If no four numbers sum up to the target sum,the function should return an empty array.

**Sample Input #1**

> array=[7,6,4,-1,1,2] <br>targetSum = 16

**Sample Output #1**

> [[7,6,4,-1],[7,6,1,2]]. // the quadruplets could be ordered differently

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Using four for loops to calculate the sums of all possible quadruplets in the array would generate an algorithm that runs in O(n^4)time,where n is the length of the input array.Can you come up with something faster using fewer for loops? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> You can calculate the sums of every pair of numbers in the array in O(n^2)time using just two for loops.Then,assuming that you've stored all of these sums in a hash table,you can fairly easily find which two sums can be paired to add up to the target sum:the numbers summing up to these two sums constitute candidates for valid quadruplets;you just have to make sure that no number was used to generate both of the two sums.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> You can do everything described in Hint #2 with just two sibling for loops nested inside a third for loop.Your goal is to create a hash table mapping the sums of every pair of numbers in the array to an array of arrays,with each subarray representing the indices of each pair summing up to that number.Loop through the input array with a simple for loop.Inside this loop,loop through the input array again,starting at the index of the first loop.At each iteration,calculate the difference between the target sum and the sum of the two numbers represented by the indices of the for loops.If that difference is in the hash table that you're building,then valid quadruplets can be formed by combining the current pair of numbers with each pair stored in the hash table at the difference just calculated.Following this nested for loop,loop through the array again,this time starting at index zero all the way to the index of the first for loop.At each iteration,calculate the sum of the two numbers represented by the indices of the for loops and add it to the hash table if it isn't already there;then add the pair of indices to the array that the sum in the hash table maps to.  </strong></i>
</details>

<br>

## Method 1


```tex
【 O(n^3)time | O(n) space 】\\
 \\
```

<br>


```tex
1.\ Array\ sort\ O(nlogn)\\
2.\ Traversal\ Array\ O(n^2)\ *\ Double\ pointer\ Traversal\ O(n)
```



```java
package Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/04/04
 */
public class FourNumberSum2 {

    public List<List<Integer>> fourSum(int[] array, int targetSum) {
        // Write your code here.
        List<List<Integer>> result = new ArrayList<List<Integer>>();

        // if array's length smaller than 4,then output result
        if(array.length < 4){
            return result;
        }

        Arrays.sort(array);
        int length = array.length;

        for(int i = 0; i < array.length - 3; i++ ){
            //skip duplicate i
            if(i > 0 && array[i] == array[i-1]){
                continue;
            }

            if ((long) array[i] + array[i + 1] + array[i + 2] + array[i + 3] > targetSum) {
                break;
            }
            if ((long) array[i] + array[length - 3] + array[length - 2] + array[length - 1] < targetSum) {
                continue;
            }


            for(int j = i + 1; j < array.length - 2; j++){
                //skip duplicate j
                if(j > i+1 && array[j] == array[j-1]){
                    continue;
                }

                if ((long) array[i] + array[j] + array[j + 1] + array[j + 2] > targetSum) {
                    break;
                }
                if ((long) array[i] + array[j] + array[length - 2] + array[length - 1] < targetSum) {
                    continue;
                }

                int newTarget = targetSum - (array[i] + array[j]);

                int k = j + 1;
                int l = array.length - 1;

                while(k < l){
                    if(array[k] + array[l] == newTarget){
                        result.add(Arrays.asList(array[i] , array[j] , array[k] , array[l]));


                        while (k < l && array[k] == array[k + 1]) {
                            k++;
                        }
                        k++;
                        while (k < l && array[l] == array[l - 1]) {
                            l--;
                        }
                        l--;

                    }else if(array[k] + array[l] < newTarget){
                        k++;
                        //while(k < l && array[k] == array[k-1]) k++;
                    }else{
                        l--;
                        //while(k < l && array[l] == array[l+1]) l--;
                    }
                }
            }
        }
        return result;
    }


}

```

