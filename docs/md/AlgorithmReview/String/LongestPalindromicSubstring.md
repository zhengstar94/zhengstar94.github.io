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
    public static String longestPalindromicSubstring(String s) {
        if (s == null || s.length() < 2) {
            return s;
        }

        // Start index of the longest palindromic substring
        int start = 0;
        // End index of the longest palindromic substring
        int end = 0;

        for (int i = 0; i < s.length(); i++) {
            // Length of palindromic substring with odd length
            int len1 = expandAroundCenter(s, i, i);
            // Length of palindromic substring with even length
            int len2 = expandAroundCenter(s, i, i + 1);
            int len = Math.max(len1, len2);

            // Update the start and end indices if a longer palindromic substring is found
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }

        // Extract the longest palindromic substring
        return s.substring(start, end + 1);
    }

    private static int expandAroundCenter(String s, int left, int right) {
        // Expand around the center and count the length of the palindromic substring
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // Return the length of the palindromic substring
        return right - left - 1;
    }


    public static void main(String[] args) {
        System.out.println(longestPalindromicSubstring("abaxyzzyxf"));
    }
}
```

