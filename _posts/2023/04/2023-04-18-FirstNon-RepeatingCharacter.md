---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "First Non-Repeating Character"
date: "2023-04-18"
categories:
  - "Algorithm String"
---

# First Non-Repeating Character [Easy]

- Write a function that takes in a string of lowercase English-alphabet letters and returns the index of the string's first non-repeating character.
- The first non-repeating character is the first character in a string that occurs only once.
- If the input string doesn't have any non-repeating characters, your function should return -1.

**Sample Input**

```
string = "abcdcaf"
```

**Sample Output**

```
1 // The first non-repeating character is "b" and is found at index 1.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> How can you determine if a character only appears once in the entire input string? What would be the brute-force approach to solve this problem? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> One way to solve this problem is with nested traversals of the string: you start by traversing the string, and for each character that you traverse, you traverse through the entire string again to see if the character appears anywhere else. The first index at which you find a character that doesn't appear anywhere else in the string is the index that you return. This approach works, but it's not optimal. Are there any data structures that you can use to improve the time complexity of this approach? </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Hash tables are very commonly used to keep track of frequencies. Build a hash table, where every key is a character in the string and every value is the corresponding character's frequency in the input string. You can traverse the entire string once to fill the hash table, and then with a second traversal through the string (not a nested traversal), you can use the hash table's constant-time lookups to find the first character with a frequency of 1. </strong></i>
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
 * @date 2023/05/17
 */
public class FirstNonRepeatingCharacter {
    public int firstNonRepeatingCharacter(String str) {
        // Assuming only lowercase English-alphabet letters
        int[] charCount = new int[26]; 

        // Count the occurrences of each character
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            charCount[c - 'a']++;
        }

        // Find the first non-repeating character
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if (charCount[c - 'a'] == 1) {
                return i;
            }
        }

        // No non-repeating characters found
        return -1; 
    }
}
```
