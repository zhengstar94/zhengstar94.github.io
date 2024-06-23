---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Zero Sum Subarray"
date: "2023-01-15"
categories:
  - "Algorithm Array"
---

# Zero Sum Subarray [Medium]

- You're given a list of integers `nums` Write a function that returns a boolean representing whether there exists a zero-sum subarray of `nums`.

- A zero-sum subarray is any subarray where all of the values add up to zero.A subarray is any contiguous section of the array.For the purposes of this problem,a subarray can be as small as one element and as long as the original array.





**Sample Input **

````
```
nums=[-5,-5,2,3,-2]
```
````

**Sample Output **

```
True //The subarray [-5,2,3]has a sum of 0
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> A good way to approach this problem is to first think of a simpler version. How would you check if the entire array sum is zero? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> If the entire array does not sum to zero, then you need to check if there are any smaller subarrays that sum to zero.For this, it can be helpful to keep track of all of the sums from [0,i], where i is every index in the array. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> After recording all sums from [0,i],what would it mean if a sum is repeated?  </strong></i>
</details>


<br>



## Method 1(SET)

```tex
【O(n)time∣O(n)space】
```

```java
package Array;

import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2023/01/25
 */
public class ZeroSumSubarray {

    public boolean zeroSumSubarray(int[] nums) {
        // Declare a HashSet that stores prefixes and
        Set<Integer> sums = new HashSet<>();
        // Add an initial value of 0 to the set
        sums.add(0);
        // Declare the prefix of the current element and
        int currentSum = 0;
        // Traversing the nums array
        for (int num : nums) {
            // Add the current element to the prefix and
            currentSum += num;
            // Detect whether the set has the same prefix and the same prefix as currentSum
            if (sums.contains(currentSum)) {
                // If so, it indicates that there is a subinterval with a sum of 0
                return true;
            }
            // No, just join set.
            sums.add(currentSum);
        }
        // After traversing, no subinterval with a sum of 0 was found
        return false;
    }
}
```

## Method 2(MAP)

```tex
【O(n)time∣O(n)space】
```

```java
package Array;

import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2023/01/25
 */
public class ZeroSumSubarray {
    public boolean zeroSumSubarray(int[] nums) {
        // Declare a Map that stores prefixes and
        Map<Integer, Integer> sumMap = new HashMap<>();
        // Add an initial value of 0 to the map
        sumMap.put(0, -1);
        // Declare the prefix of the current element and
        int currentSum = 0;
        // Traversing the nums array
        for (int i = 0; i < nums.length; i++) {
            // Add the current element to the prefix and
            currentSum += nums[i];
            // Detect whether the map has the same prefix and the same prefix as currentSum
            if (sumMap.containsKey(currentSum)) {
                // If so, it indicates that there is a subinterval with a sum of 0
                return true;
            }
            // No, just join map.
            sumMap.put(currentSum, i);
        }
        // After traversing, no subinterval with a sum of 0 was found
        return false;
    }
}

```






