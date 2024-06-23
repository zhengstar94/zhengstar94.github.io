---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Longest Palindromic Substring"
date: "2023-04-20"
categories:
  - "Algorithm String"
---

# Longest Palindromic Substring [Medium]

- Write a function that, given a string, returns its longest palindromic substring.
- A palindrome is defined as a string that's written the same forward and backward. Note that single-character strings are palindromes.
- You can assume that there will only be one longest palindromic substring.

**Sample Input**

```
string = "abaxyzzyxf"
```

**Sample Output**

```
"xyzzyx"
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try generating all possible substrings of the input string and checking for their palindromicity. What is the runtime of the isPalindrome check? What is the total runtime of this approach? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Recognize that a palindrome is a string that is symmetrical with respect to its center, which can either be a character (in the case of odd-length palindromes) or an empty string (in the case of even-length palindromes). Thus, you can check the palindromicity of a string by simply expanding from its center and making sure that characters on both sides are indeed mirrored. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Traverse the input string, and at each index, apply the logic mentioned in Hint #2. What does this accomplish? Is the runtime of this approach better? </strong></i>
</details>


<br>

## Method 1

```tex
【O(n^2)time∣O(1)space】
```

```java
package String;

/**
 * @author zhengstars
 * @date 2023/05/21
 */
public class LongestPalindromicSubstring {
    public static String longestPalindromicSubstring(String str) {
        // Store the indices of the current longest palindromic substring
        int[] currentLongest = new int[] {0, 1};

        // Iterate through each character in the string
        for (int i = 1; i < str.length(); i++) {
            // Find the longest palindrome with odd length
            int[] odd = findLongestPalindrome(str, i - 1, i);

            // Find the longest palindrome with even length
            int[] even = findLongestPalindrome(str, i - 1, i + 1);

            // Determine the longest palindrome between odd and even length palindromes
            int[] longest = even[1] - even[0] > odd[1] - odd[0] ? even : odd;

            // Update the current longest palindrome if a longer one is found
            currentLongest = longest[1] - longest[0] > currentLongest[1] - currentLongest[0] ? longest : currentLongest;
        }

        // Return the longest palindromic substring
        return str.substring(currentLongest[0], currentLongest[1]);
    }

    // Helper function to find the longest palindromic substring given the left and right indices
    public static int[] findLongestPalindrome(String str, int leftIdx, int rightIdx) {
        // Expand outwards from the indices until the palindrome condition is no longer satisfied
        while (leftIdx >= 0 && rightIdx < str.length()) {
            if (str.charAt(leftIdx) != str.charAt(rightIdx)) {
                break;
            }
            leftIdx--;
            rightIdx++;
        }

        // Return the indices of the longest palindromic substring
        return new int[] {leftIdx + 1, rightIdx};
    }



    public static void main(String[] args) {
        System.out.println(longestPalindromicSubstring("abaxyzzyxf"));
    }
}

```

