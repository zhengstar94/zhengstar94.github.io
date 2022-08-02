# Three Number Sum

- Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all triplets in the array that sum up to the target sum and return a two-dimensional array of all these triplets. The numbers in each triplet should be ordered in ascending order, and the triplets themselves should be ordered in ascending order with respect to the numbers they hold.

- lf no three numbers sum up to the target sum, the function should return an empty array.

**Sample Input**

> array = [12,3,1,2,-6,5,-8,6]<br>targetSum = 0

**Sample Output**

> [[-8,2,6], [-8,3,5], [-6,1,5]]

## Method1

```tex
【 O(n^3)time | O(n) space 】
```



```java
import java.util.*;

class Program {
  public static List<Integer[]> threeNumberSum(int[] array, int targetSum) {
    // Write your code here.
    Arrays.sort(array);
    
    List<Integer[]> list = new ArrayList<Integer[]>();

    for(int i = 0; i < array.length; i++){
      for(int j = i+1; j < array.length; j++){
        for(int k = j+1; k < array.length; k++){
          if(array[i]+array[j]+array[k] == targetSum){
            list.add(new Integer[]{array[i],array[j],array[k]});
          }
        }
      }   
    }
    return list;
  }
}
```



## Method2

```tex
【 O(n^2)time | O(n) space 】\\
 \\
```

<br>

```tex
1.\ Array\ sort\ O(nlogn)\\
2.\ Traversal\ Array\ O(n)\ *\ Double\ pointer\ Traversal\ O(n)
```


```java
import java.util.*;

class Program {
  public static List<Integer[]> threeNumberSum(int[] array, int targetSum) {
    // Write your code here.
    Arrays.sort(array);
    
    List<Integer[]> list = new ArrayList<Integer[]>();


    for(int i = 0; i < array.length; i++){
      int sum = targetSum - array[i];
      
      int left = i+1;
      int right = array.length-1;

      while(left<right){
        if(array[left] + array[right] == sum){
          list.add(new Integer[]{array[i],array[left],array[right]});
          left++;
          right--;
        }else if(array[left] + array[right] < sum){
          left++;
        }else{
          right--;
        }
      }
    } 
    return list;
  }
}
```

