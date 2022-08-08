# Array Of Products [Medium]

- Write a function that takes in a non-empty array of integers and returns an array of the same length, where each element in the output array is equal to the product of every other number in the input array.
- In other words, the value at `output [i]`is equal to the product of every number in the input array other than `input[i]`.
- Note that you're expected to solve this problem without using division.

**Sample Input**

> array=[5,1,4,2]

**Sample Output**

> [8,40,10,20]
>
> // 8 is equal to 1 x 4 x 2
>
> // 40 is equal to 5 x 4 x 2
>
> // 10 is equal to 5 x 1 x 2
>
> // 20 is equal to 5 x 1 x 4



## Method 1



```tex
【 O(n^2) time | O(n) space 】
```



```java
import java.util.*;

class Program {
  public int[] arrayOfProducts(int[] array) {
    // Write your code here.
    
    int[] temp = new int[array.length];
    for(int i = 0; i < array.length; i++){
      int sum = 1;
      for(int j = 0; j < array.length; i++){
          if(i != j){
              sum *= array[j];
          }
      }
      temp[i] = sum;
      
    }
    return temp;
  }
}

```

## Method 2



```tex
【 O(n) time | O(n) space 】
```



```java
import java.util.*;

class Program {
  public int[] arrayOfProducts(int[] array) {
    // Write your code here.
    int[] temp = new int[array.length];

    for(int i = 0; i < array.length; i++ ){
      int leftProduct = 1;
      int rightProduct = 1;
      int leftIdx = 0;
      int rightIdx = array.length - 1;

      while(leftIdx >= 0 && leftIdx < i ){
        leftProduct *= array[leftIdx];
        leftIdx++;
      }
      
      while(rightIdx > i && rightIdx <=  array.length - 1){
        rightProduct *= array[rightIdx];
        rightIdx--;
      }


      temp[i] = (leftProduct*rightProduct);
    }

    
    return temp;
  }
}

```

