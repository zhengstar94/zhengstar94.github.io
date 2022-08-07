# Longest Peak [Medium]

- Write a function that takes in an array of integers and returns the length of the longest peak in the array.
- A peak is defined as adjacent integers in the array that are strictly increasing until they reach a tip (the highest value in the peak), at which point they become strictly decreasing. At least three integers are required to form a peak.
- For example,the integers `1,4,10,2` form a peak,but the integers `4,0,10` don't and neither do the integers `1,2,2,0`.  Similarly, the integers `1,2,3` don't form a peak because there aren't any strictly decreasing integers after the `3`.

**Sample Input**

> array=[1,2,3,3,4,0,10,6,5,-1,-3,2,3]

**Sample Output**

> 6//0,10,6,5,-1,-3



## Method 1



```tex
【O(n)time\ N\ is\ the\ length\ of\ the array\ ∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static int longestPeak(int[] array) {
    // Write your code here.
    int longestNumber = 0;

    int arrayLength = array.length - 1;
    
    if(array.length < 3){
      return longestNumber;
    }

    for(int i = 1; i < arrayLength; i++){
      if(array[i] > array[i - 1] && array[i] > array[i + 1]){
          int left = i - 1;
          int right = i + 1;

          //left boundary
          while(left > 0 && array[left] > array[left-1]){
            left--;
          }

          //right boundary
          while(right < arrayLength && array[right] > array[right+1]){
            right++;
          }

          longestNumber = Math.max(longestNumber, right-left+1);
        
      }
    }
     
    return longestNumber;
  }
}

```

