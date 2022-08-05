# Move Element To End[Medium]

- You're given an array of integers and an integer. Write a function that moves all instances of that integer in the array to the end of the array and returns the array.
- The function should perform this in place (i.e., it should mutate the input array)and doesn't need to maintain the order of the other integers.

**Sample Input**

> array=[2,1,2,2,2,3,4,2] <br>
> toMove = 2

**Sample Output**

> [1,3,4,2,2,2,2,2] // the numbers 1,3,and 4 could be ordered differently



## Method 1

```tex
【O(n)time∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static List<Integer> moveElementToEnd(List<Integer> array, int toMove) {
    // Write your code here.
    int count = 0;
    for(int i = 0; i < array.size(); i++){
      if(array.get(i)!=toMove){
        array.set(count++,array.get(i));
      }
    }

    while(count < array.size()){
      array.set(count++,toMove);
    }
    return array;
  }
}
```



## Method 2

## 

```tex
【O(n)time∣O(1)space】
```



```java
import java.util.*;

class Program {
  public static List<Integer> moveElementToEnd(List<Integer> array, int toMove) {
    // Write your code here.
    int left = 0;
    int right = array.size() - 1;

    while(left < right){
      //  left < right [ because prevent right < left, if you don't add this,your algorithm will break.]
      while(left < right && array.get(right) == toMove){
        right --;
      }

      if(array.get(left) == toMove){
        int temp = array.get(right);
        array.set(right,array.get(left));
        array.set(left,temp);

      }
      left++;
    }

    return array;
  }
}
```

