# Monotonic Array[Medium]

- Write a function that takes in an array of integers and returns a boolean representing whether the array is monotonic.
- An array is said to be monotonic if its elements,from left to right,are entirely non-increasing or entirely non-decreasing.
- Non-increasing elements aren't necessarily exclusively decreasing;they simply don't increase. Similarly, non-decreasing elements aren't necessarily exclusively increasing;they simply don't decrease.
- Note that empty arrays and arrays of one element are monotonic.

**Sample Input**

> array=[-1,-5,-10,-1100,-1100,-1101,-1102,-9001]

**Sample Output**

> true



## Method 1

```tex
【O(n)time∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static boolean isMonotonic(int[] array) {
    // Write your code here.
    int length = array.length;
    
    if(length <= 1){
      return true;
    }
    boolean isIncreasing = false;
    boolean isDecreasing = false;
    
    for(int i = 1; i < length; i++){
      if(array[i] > array[i-1]){
        isDecreasing = true;
      }

      if(array[i] < array[i-1]){
        isIncreasing = true;
      }

      //if it exists isIncreasing and isDecreasing at the same time, return false 
      if(isIncreasing && isDecreasing){
        return false;
      }
    }
    return true;
  }
}
```



## Method 2

```tex
【O(n)time∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static boolean isMonotonic(int[] array) {
    // Write your code here.
    int length = array.length;
    
    if(length <= 1){
      return true;
    }

    // default increasing or decreasing
    // true  increasing
    // false decreasing
    boolean flag =  array[0] > array[length - 1] ;

    for(int i = 1; i < length; i++){
      // if array is default increasing
      // then if exist 'array[i] > array[i-1]' that means decreasing
      // then return false
      if(flag && array[i] > array[i-1]){
        return false;
      }

      // if array is default decreasing
      // then if exist 'array[i] < array[i-1]' that means increasing
      // then return false
      if(!flag && array[i] < array[i-1]){
        return false;
      }
    }
    return true;
  }
}
```

