---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Palindrome Check"
date: "2023-04-13"
categories:
  - "Algorithm String"
---

# Palindrome Check [Easy]

- Write a function that takes in a non-empty string and that returns a boolean representing whether the string is a palindrome.
- A palindrome is defined as a string that's written the same forward and backward. Note that single-character strings are palindromes.

**Sample Input**

```
string = "abcdcba"
```

**Sample Output**

```
true // it's written the same forward and backward
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Start by building the input string in reverse order and comparing this newly built string to the input string. Can you do this without using string concatenations? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Can you optimize your algorithm by using recursion? What are the implications of recursion on an algorithm's space-time complexity analysis. </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Go back to an iterative solution and try using pointers to solve this problem: start with a pointer at the first index of the string and a pointer at the final index of the string. What can you do from there? </strong></i>
</details>


<br>

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package String;

/**
 * @author zhengstars
 * @date 2023/05/14
 */
public class PalindromeCheck {
    public static boolean isPalindrome(String str) {
        // Initialize two pointers, one at the beginning and one at the end of the string
        int left = 0;
        int right = str.length() - 1;

        // Loop through the string until the two pointers meet or cross
        while (left < right) {
            // If the characters at the left and right pointers are not equal, return false
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            // Move the left pointer to the right and the right pointer to the left
            left++;
            right--;
        }
        // If the loop completes without returning false, the string is a palindrome
        return true;
    }

    public static void main(String[] args) {
        String str = "BAB";
        System.out.println(isPalindrome(str));
    }
}
```

