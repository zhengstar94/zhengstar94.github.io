# Largest Range [Hard]

- Write a function that takes in an array of integers and returns an array of length 2 representing the largest range of integers contained in that array.
- The first number in the output array should be the first number in the range, while the second number should be the last number in the range.
- A range of numbers is defined as a set of numbers that come right after each other in the set of real integers. For instance, the output array `[2,6]`represents the range`{2,3,4,5,6}`, which is a range of length 5. Note that numbers don't need to be sorted or adjacent in the input array in order to form a range.
- You can assume that there will only be one largest range.

**Sample Input #1**

> array=[1,11,3,0,15,5,2,4,10,7,12,6]

**Sample Output #1**

> [0,7]



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
        //array[i] == array[i+1] equal or array[i] + 1 == array[i+1] equal, it means continuous.
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



