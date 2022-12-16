# Four Number Sum [Hard]

- Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all quadruplets in the array that sum up to the target sum and return a two-dimensional array of all these quadruplets in no particular order.
- If no four numbers sum up to the target sum,the function should return an empty array.

**Sample Input #1**

> array=[7,6,4,-1,1,2] <br>targetSum = 16

**Sample Output #1**

> [[7,6,4,-1],[7,6,1,2]]. // the quadruplets could be ordered differently



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
import java.util.*;

class Program {
  public static List<Integer[]> fourNumberSum(int[] array, int targetSum) {
    // Write your code here.
    List<Integer[]> result = new ArrayList<Integer[]>();

    // if array's length smaller than 4,then output result
    if(array.length < 4){
      return result;
    }

    Arrays.sort(array);

    for(int i = 0; i < array.length - 3; i++ ){
        //skip duplicate i
        if(i > 0 && array[i] == array[i-1]){
          continue;
        }

        // If the first four numbers are greater than the target value, exit 
        if ((long) array[i] + array[i + 1] + array[i + 2] + array[i + 3] > targetSum) {
            break;
        }

        // If the sum of the first number and the last three numbers is less than the target value, exit the current cycle and proceed to the next round
        if ((long) array[i] + array[length - 3] + array[length - 2] + array[length - 1] < targetSum) {
            continue;
        }

        for(int j = i + 1; j < array.length - 2; j++){

          //skip duplicate j
          if(j > i+1 && array[j] == array[j-1]){
            continue;
          }

          // If the first four numbers are greater than the target value, exit 
            if ((long) array[i] + array[j] + array[j + 1] + array[j + 2] > targetSum) {
                break;
            }

            // If the sum of the first number and the last three numbers is less than the target value, exit the current cycle and proceed to the next round
            if ((long) array[i] + array[j] + array[length - 2] + array[length - 1] < targetSum) {
                continue;
            }

          int newTarget = targetSum - (array[i] + array[j]);
          
          int k = j + 1;
          int l = array.length - 1;
          while(k < l){
            
            if(array[k] + array[l] == newTarget){
              result.add(new Integer[]{array[i] , array[j] , array[k] , array[l]});

                //skip duplicate k
                while (k < l && array[k] == array[k + 1]) {
                    k++;
                }
                k++;

                //skip duplicate l
                while (k < l && array[l] == array[l - 1]) {
                    l--;
                }
                l--;
              //while(k < l && array[k] == array[k-1]) k++;
              //while(k < l && array[l] == array[l+1]) l--;
              
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

