
# Binary Search

> 一种针对有序区间内的O(logN)搜索方式，最常见用于已经排好序的Array

## Binary Search两大基本原则

1. 每次都要缩减搜索区域
   Shrink the search space every iteration (or recursion)
2. 每次缩减不能排除潜在答案
   Cannot exclude potential answers during each shrinking

## Binary Search三大模板

- **找一个准确值**
  - 循环条件：left <= right
  - 缩减搜索空间：left = mid+1，right = mid-1
- **找一个模糊值**(比4大的最小数)
  - 循环条件：left < right
  - 缩减搜索空间：left = mid，right = mid-1 或者 left = mid+1，right = mid
- **万用型**
  - 循环条件：left < right -1
  - 缩减搜索空间：left = mid，right = mid

#### 模板实践

##### 1. 找一个模糊值

循环条件：left < right

缩减搜索空间：left = mid，right = mid-1 或者 left = mid+1，right = mid

###### (First Occurance of 2 找第一个2出现的位置）只能使用  left = mid+1，right = mid

[1,1,2,2,2,6,7]

```java
public int binarySearch(int[] arr,int k){
	int left = 0, right = arr.length - 1;
	while (left < right){
		int mid = left + (right - left) / 2;
		if (arr[mid] < k){
			left=mid +1;
        }else {
			right=mid;
        }
    }
	return left;
}
```



###### (Last Occurance of 2 找最后一个2出现的位置）只能使用  left = mid，right = mid-1

[1,1,2,2,2,6,7]

```java
public int binarySearch(int[] arr,int k){
	int left = 0, right = arr.length - 1;
	while (left < right){
        //此处的区别是，如果是偶数个的话，先把中间值放在右边
		int mid = left + (right - left + 1) / 2;
		if (arr[mid] < k){
			left=mid ;
        }else {
			right=mid - 1;
        }
    }
	return left;
}
```



##### 万用型

###### Closest to 2，最接近2的数

循环条件：left < right -1

缩减搜索空间：left = mid，right = mid 

```java
public int binarySearch(int[] arr,int k){
	int left = 0, right = arr.length - 1;
	while (left < right-1){
		int mid = left + (right - left) / 2;
		if (arr[mid] < k){
			left=mid ;
        }else {
			right=mid;
        }
    }
    
    if(arr[right]<k){
        return right
    } else if(arr[left] > k ){
        return left
    } else{
        return k - arr[right] < arr[right] - k ? left : right;
    } 
}
```



#### 理论实践

##### 有序区间，力扣1062题；Longest Repeating Substring

给定字符串 S，找出**最长重复子串**的`长度`。如果不存在重复子串就返回 0。

> 示例 1：
> 输入："abcd"
> 输出：0
> 解释：没有重复子串。
>
> 示例 2：
> 输入："abbaba"
> 输出：2
> 解释：最长的重复子串为 "ab" 和 "ba"，每个出现 2 次。
>
> 示例 3：
> 输入："aabcaabdaab"
> 输出：3
> 解释：最长的重复子串为 "aab"，出现 3 次。
>
> 示例 4：
> 输入："aaaaa"
> 输出：4
> 解释：最长的重复子串为 "aaaa"，出现 2 次。
>
> 提示：
> 字符串 S 仅包含从 'a' 到 'z' 的小写英文字母。
> 1 <= S.length <= 1500

###### 暴力查找O(n^3)

- 所有substring->O(n^2)
- HashSet查重substring->O(n)

```java
    /**
     * 1.比如aabcaabdaab，长度为11，mid为5的长度
     * 2.调用f函数，依次遍历aabcaabdaab，每次取5个字符，第一次aabca，如果不存在则放入set中，之后遍历是abcaa，依次类推
     * 3.发现没有符合的数据，f函数返回false，说明长度为5不存在重复字符串，
     * 4.即right=mid-1，right = 4， 循环，二分法找到mid=2，长度为2，
     * 5.调用f函数，返回true，说明长度为2的数据存在重复，即left = mid ，循环，二分法 在2与4之间找到mid= 3，长度为3
     * 6.调用f函数，返回true，说明长度为3的数据存在重复，即left = mid，循环，二分法 在3与4之间找到mid= 4，长度为4
     * 7.调用f函数，返回true，说明长度为4的数据不存在重复，即right = mid -1 为3 ，循环发现left(3)<right(3)不成立
     * 8.跳出循环返回left为3
     * @param s
     * @return
     */
    public static int longestRepeatingsubstring(String s) {
        int left = 0, right = s.length() - 1;
        while(left < right) {
            int mid = left + (right - left + 1) / 2;
            if (f(s, mid)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;

    }

    /**
     * @param s
     * @param length
     * @return
     */
    public static boolean f(String s, int length) {
        Set<String> seen = new HashSet<>();
        for (int i = 0; i <= s.length() - length; i++) {
            //通过length依次往后截取字符串
            int j = i + length - 1;
            String sub = s.substring(i, j + 1);
            if (seen.contains(sub)) {
                return true;
            }
            seen.add(sub);
        }
        return false;
    }
```



#### 注意点

1. 溢出问题

```java
int mid = (left + right)/2;
//使用下面这个方式找中间值，上面的方式可能会溢出
int mid = left + (right - left)/2;
```



