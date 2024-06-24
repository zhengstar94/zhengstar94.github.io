---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "One Edit Distance"
date: "2023-04-25"
categories:
  - "Algorithm String"
---

# One Edit Distance [Medium]

- Given two strings s and t, determine if they are both one edit distance apart.
- Note: There are 3 possiblities to satisify one edit distance apart:
  - Insert a character into s to get t
  - Delete a character from s to get t
  - Replace a character of s to get t


**Sample Input 1**

```
s = "ab", t = "acb"
```

**Sample Output 1**

```
true //We can insert 'c' into s to get t.
```

**Sample Input 2**

```
s = "cab", t = "ad"
```

**Sample Output 2**

```
false //We cannot get t from s by only one step.
```

**Sample Input 3**

```
s = "1203", t = "1213"
```

**Sample Output 3**

```
true //We can replace '0' with '1' to get t.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> There are a few different ways to solve this problem, but all of them use the same general approach. You'll need to determine not only all of the unique characters required to form the input words, but also their required frequencies. What determines the required frequencies of characters to form multiple words? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The word that contains the highest frequency of any character dictates how many of those characters are required. For example, given words = ["A", "AAAA"] you need 4 As, because the word that contains the most of amount of As has 4. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Use a hash table to keep track of the maximum frequencies of all unique characters that occur across all words. Count the frequency of each character in each word, and use those per-word frequencies to update your maximum-character-frequency hash table. Once you've determined the maximum frequency of each character across all words, you can use the built-up hash table to generate your output array. </strong></i>
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
 * @date 2023/05/27
 */
public class OneEditDistance {
    public static boolean isOneEditDistance(String s, String t) {

        // If s or t is null, return false because they cannot be compared.
        if (s == null || t == null) {
            return false;
        }

        // If the difference in length between s and t is greater than 1, return false because the distance cannot exceed 1.
        int m = s.length(), n = t.length();
        if (Math.abs(m - n) > 1) {
            return false;
        }

        // Initialize pointers
        int i = 0;
        int j = 0;

        // Initialize counter 'count' to 0, which counts the number of different characters.
        int count = 0;

        while (i < m && j < n) {
            if (s.charAt(i) == t.charAt(j)) {
                // Characters are the same, continue comparing the next character.
                i++;
                j++;
            } else {
                count++; // Characters are different, increment edit distance by one.
                if (count > 1) {
                    // Edit distance exceeds 1, does not meet the condition.
                    return false;
                }
                if (m == n) {
                    // Strings have the same length, continue comparing by replacing the character.
                    i++;
                    j++;
                } else if (m > n) {
                    // s is longer than t, continue comparing by deleting a character from s.
                    i++;
                } else {
                    // s is shorter than t, continue comparing by deleting a character from t.
                    j++;
                }
            }
        }
        if (i < m || j < n) {
            // If there are remaining characters, increment the edit distance by one.
            count++;
        }
        // Check if the edit distance is 1.
        return count == 1;
    }

    public static void main(String[] args) {
        System.out.println(isOneEditDistance("cab","ab"));
    }
}
```

