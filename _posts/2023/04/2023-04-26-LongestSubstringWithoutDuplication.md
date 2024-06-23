---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Longest Substring Without Duplication"
date: "2023-04-26"
categories:
  - "Algorithm String"
---

# Longest Substring Without Duplication [Hard]

- Write a function that takes in a string and returns its longest substring without duplicate characters.
- You can assume that there will only be one longest substring without duplication.

**Sample Input **

```
clementisacap
```

**Sample Output **

```
mentisac
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try traversing the input string and storing the last position at which you see each character in a hash table. How can this help you solve the given problem? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> As you traverse the input string, keep track of a starting index variable. This variable, as its name suggests, should represent the most recent index from which you could start a substring with no duplicate characters, ending at your current index. Use the hash table mentioned in Hint #1 to update this variable correctly, and update the longest substring as you go.</strong></i>
</details>



<br>

## Method 1

```tex
【O(n)time∣O(min(n, a))space】
```

```tex
where n is the length of the input string and a is the length of the character alphabet represented in the input string
```

```java
package String;

import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2023/06/01
 */
public class LongestSubstringWithoutDuplication {

    public static String longestSubstringWithoutDuplication(String str) {
        // Store the last seen index of characters
        Map<Character, Integer> lastSeen = new HashMap<>();
        // Start and end indices of the longest substring
        int[] longest = new int[]{0, 1};
        // Start index of the current substring
        int startIndex = 0;

        for (int i = 0; i < str.length(); i++) {
            char currentChar = str.charAt(i);

            if (lastSeen.containsKey(currentChar)) {
                // Update the start index
                startIndex = Math.max(startIndex, lastSeen.get(currentChar) + 1);
            }

            if (longest[1] - longest[0] < i + 1 - startIndex) {
                // Update the start index of the longest substring
                longest[0] = startIndex;
                // Update the end index of the longest substring
                longest[1] = i + 1;
            }

            // Update the last seen index of the character
            lastSeen.put(currentChar, i);
        }

        // Get the longest substring
        return str.substring(longest[0], longest[1]);
    }

    public static void main(String[] args) {
        String input = "clementisacap";
        String result = longestSubstringWithoutDuplication(input);
        System.out.println(result);
    }
}

```

