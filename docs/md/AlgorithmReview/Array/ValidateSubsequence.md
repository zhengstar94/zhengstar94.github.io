# Validate Subsequence[Easy]

- Given two non-empty arrays of integers,write a function that determines whether the second array is a subsequence of the first one.

- A subsequence of an array is a set of numbers that aren't necessarily adjacent in the array but that are in the same order as they appear in the array. For instance,the numbers `[1,3,4]`form a subsequence of the array `[1,2,3,4]`,and so do the numbers `[2,4]`Note that a single number in an array and the array itself are both valid subsequences of the array.

**Sample Input**
> array=[5,1,22,25,6,-1,8,10] <br>
> sequence=[1,6,-1,10]

**Sample Output**
> true


## Method 1  

```tex 
【 O(n)time | O(1)space 】
```

```java
import java.util.*;

class Program {
  public static boolean isValidSubsequence(List<Integer> array, List<Integer> sequence) {
    int index = 0;
    for(int i = 0; i < array.size(); i++){
        if(array.get(i) == sequence.get(index)){
          index++;
        }

        if(index == sequence.size()){
          return true;
        }
    }
    return false; 
  }
}
```

## Method 2

```tex 
【 O(n)time | O(1)space 】
```

```java
import java.util.*;

class Program {
    public static boolean isValidSubsequence(List<Integer> array, List<Integer> sequence) {
        // Write your code here.
        int arrIndex = 0;
        int index = 0;

        while(arrIndex < array.size() && index < sequence.size()){
            if(array.get(arrIndex) == sequence.get(index)){
                index++;
            }
            arrIndex++;
        }
        return sequence.size() == index;
    }
}

```