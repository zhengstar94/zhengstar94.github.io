# Sorted Squared Array[Easy]

Write a function that takes in a non-empty array of integers that are sorted in ascending order and returns a new array of the same length with the squares of the original integers also sorted in ascending order.

**Sample Input**

> array=[1,2,3,5,6,8,9]

**Sample Output**

> [1,4,9,25,36,64,81]



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

## Method 1

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

