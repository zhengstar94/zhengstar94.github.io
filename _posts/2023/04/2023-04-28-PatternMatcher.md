---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Pattern Matcher"
date: "2023-04-28"
categories:
  - "Algorithm String"
---

# Pattern Matcher [Hard]

- You're given two non-empty strings. The first one is a pattern consisting of only "x"s and / or "y"s; the other one is a normal string of alphanumeric characters. Write a function that checks whether the normal string matches the pattern.
- A string S0 is said to match a pattern if replacing all "x"s in the pattern with some non-empty substring S1 of S0 and replacing all "y"s in the pattern with some non-empty substring S2 of S0 yields the same string S0.
- If the input string doesn't match the input pattern, the function should return an empty array; otherwise, it should return an array holding the strings S1 and S2 that represent "x" and "y" in the normal string, in that order. If the pattern doesn't contain any "x"s or "y"s, the respective letter should be represented by an empty string in the final array that you return.
- You can assume that there will never be more than one pair of strings S1 and S2 that appropriately represent "x" and "y" in the normal string.

**Sample Input **

```
pattern = "xxyxxy"
string = "gogopowerrangergogopowerranger"
```

**Sample Output **

```
["go", "powerranger"]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Start by checking if the pattern starts with an "x". If it doesn't, consider generating a new pattern that swaps all "x"s for "y"s and vice versa; this might greatly simplify the rest of your algorithm. Make sure to keep track of whether or not you do this swap, as your final answer will be affected by it. </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Use a hash table to store the number of "x"s and "y"s that appear in the pattern, and keep track of the position of the first "y". Knowing how many "x"s and "y"s appear in the pattern, paired with the length of the main string which you have access to, will allow you to quickly test out various possible lengths for "x" and "y". Knowing where the first "y" appears in the pattern will allow you to actually generate potential substrings. </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Traverse the main string and try different combinations of substrings that could represent "x" and "y". For each potential combination, map the new pattern mentioned in Hint #1 and see if it matches the main string. </strong></i>
</details>




<br>

## Method 1

```tex
【O(n^2 + m)time∣O(n + m)space】
```

```java
package Sorting;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/06/03
 */
public class PatternMatcher {
    public static String[] patternMatcher(String pattern, String string) {
        // If the pattern or string is empty, return an empty array
        if (pattern.length() == 0 || string.length() == 0) {
            return new String[0];
        }

        // Convert the pattern string to a character array
        char[] patternArr = pattern.toCharArray();
        // Check if the pattern characters need to be swapped
        boolean swap = patternArr[0] == 'y';
        if (swap) {
            // If swapping is needed, replace all 'x' with 'y' and vice versa
            for (int i = 0; i < patternArr.length; i++) {
                if (patternArr[i] == 'x') {
                    patternArr[i] = 'y';
                } else {
                    patternArr[i] = 'x';
                }
            }
        }

        // Count the number of 'x' and 'y' in the pattern and record the index of the first 'y'
        int countX = 0;
        int countY = 0;
        int firstYIndex = -1;

        for (char c : patternArr) {
            if (c == 'x') {
                countX++;
            } else {
                countY++;
                if (firstYIndex == -1) {
                    firstYIndex = countY - 1;
                }
            }
        }

        // If the pattern contains only 'x', divide the string into appropriate lengths and return the result array
        if (countY == 0) {
            int lenX = string.length() / countX;
            String x = string.substring(0, lenX);
            String[] result = new String[2];
            result[0] = x;
            result[1] = "";
            return result;
        }

        // Try different length combinations to match the pattern
        for (int lenX = 1; lenX <= string.length() / countX; lenX++) {
            int lenY = (string.length() - lenX * countX) / countY;
            // If the string length cannot be divided correctly into the lengths of 'x' and 'y', continue to the next iteration
            if (lenY * countY != string.length() - lenX * countX) {
                continue;
            }

            // Get the indexes of the first 'x' and 'y' in the pattern
            int startX = pattern.indexOf('x');
            int startY = pattern.indexOf('y');

            // Extract substrings as 'x' and 'y' based on the partition lengths
            String x = string.substring(startX * lenY, startX * lenY + lenX);
            String y = string.substring(startY * lenX, startY * lenX + lenY);

            // Build a new string and compare it with the original string
            StringBuilder sb = new StringBuilder();
            for (char c : patternArr) {
                if (c == 'x') {
                    sb.append(x);
                } else {
                    sb.append(y);
                }
            }

            // If the constructed string is equal to the original string, a match is found
            if (sb.toString().equals(string)) {
                // Determine the order of 'x' and 'y' in the result array based on the swap flag
                String[] result = new String[2];
                result[0] = swap ? y : x;
                result[1] = swap ? x : y;
                return result;
            }
        }

        // If no match is found, return an empty array
        return new String[0];
    }

    public static void main(String[] args) {
        String pattern = "xxyxxy";
        String string = "gogopowerrangergogopowerranger";
        String[] result = patternMatcher(pattern, string);
        System.out.println(Arrays.toString(result));
    }
}
```

