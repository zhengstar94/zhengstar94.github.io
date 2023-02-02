# Sorted Squared Array[Easy]

- Write a function that takes in a non-empty array of integers that are sorted in ascending order and returns a new array of the same length with the squares of the original integers also sorted in ascending order.

**Sample Input**

> array=[1,2,3,5,6,8,9]

**Sample Output**

> [1,4,9,25,36,64,81]


**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> While the integers in the input array are sorted in increasing order,their squares won't necessarily be as well,because of the possible presence of negative numbers.</strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Traverse the array value by value,square each value,and insert the squares into an output array.Then,sort the output array before returning it.Is this the optimal solution? </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> To reduce the time complexity of the algorithm mentioned in Hint #2,you need to avoid sorting the ouput array.To do this,as you square the values of the input array,try to directly insert them into their correct position in the output array. </strong></i>
</details>

<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Use two pointers to keep track of the smallest and largest values in the input array.Compare the absolute values of these smallest and largest values,square the larger absolute value,and place the square at the end of the output array, filling it up from right to left.Move the pointers accordingly,and repeat this process until the output array is filled. </strong></i>
</details>

<br>



## Method 1

```tex 
【 O(nlog(n)) time | O(n) space 】
```


```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] temp = new int[nums.length];
        
        for(int i = 0; i < nums.length; i++){
            temp[i] = nums[i] * nums[i];
        }
        
       Arrays.sort(temp);
       return temp;
    }
}
```

## Method 2

```tex 
【 O(n) time | O(n) space 】
```



```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        int[] temp = new int[nums.length];
        
        for(int i = right;i>=0;i--){
            
            if(Math.abs(nums[left]) > Math.abs(nums[right])){
                temp[i] = nums[left] * nums[left];
                left ++;
            }else{
                temp[i] = nums[right] * nums[right];
                right --;
            }
        }
       return temp;
    }
}
```

