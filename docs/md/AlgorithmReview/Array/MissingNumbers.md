# Missing Numbers

- You're given an unordered list of unique integers `nums` in the range `[1,n]`, where `n` represents the length of `nums+2`. This means that two numbers in this range are missing from the list.
- Write a function that takes in this list and returns a new list with the two missing numbers, sorted numerically.

**Sample Input**

```
nums=[1,4,3]
```

**Sample Output**

```
[2,5] //n is 5,meaning the completed list should be [1,2,3,4,5]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> How would you solve this problem if there was only one missing number? Can that solution be applied to this problem with two missing numbers? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> To efficiently find a single missing number,you can sum up all of the values in the array as well as sum up all of the values in the expected array (i.e. in the range [1,n]). The difference between these values is the missing number. </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Using the same logic as for a single missing number, you can find the total of the two missing numbers. How can you then find which numbers these are? </strong></i>
</details>


<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> If you take an average of the two missing numbers,one of the missing numbers must be less than that average, and one must be greater than the average. </strong></i>
</details>


<br>

<details> <summary><b>Hint 5</b></summary>
    <br>
    <i><strong> Since we know there is one missing number on each side of the average, we can treat each side of the list as its own problem to find one missing number in that list. </strong></i>
</details>


<br>

## Method 1

```tex
【O(d * (n+k))time∣O(n+k)space】
```

```java

```

