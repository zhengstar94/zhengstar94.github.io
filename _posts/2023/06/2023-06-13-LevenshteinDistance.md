---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Levenshtein Distance"
date: "2023-06-13"
categories:
  - "Algorithm Dynamic"
---


# Levenshtein Distance [Medium]

- Write a function that takes in two strings and returns the minimum number of edit operations that need to be performed on the first string to obtain the second string.
- There are three edit operations: insertion of a character, deletion of a character, and substitution of a character for another.

**Sample Input**

```
str1 = "abc"
str2 = "yabd"
```

**Sample Output**

```
2 // insert "y"; substitute "c" for "d"
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Try building a two-dimensional array of the minimum numbers of edits for pairs of substrings of the input strings. Let the rows of the array represent substrings of the second input string str2. Let the first row represent the empty string. Let each row i thereafter represent the substrings of str2 from 0 to i, with i excluded. Let the columns similarly represent the first input string str1. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Build up the array mentioned in Hint #1 one row at a time. In other words, find the minimum numbers of edits between all the substrings of str1 represented by the columns and the empty string represented by the first row, then between all the substrings of str1 represented by the columns and the first letter of str2 represented by the second row, etc., until you compare both full strings. Find a formula that relates the minimum number of edits at any given point to previous numbers. </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>At any position (i, j) in the two-dimensional array, if str2[i] is equal to str1[j], then the edit distance at position (i, j) is equal to the one at position (i - 1, j - 1), since adding str2[i] and str1[j] to the substrings represented at position (i - 1, j - 1) does not require any additional edit operation. If str2[i] is not equal to str1[j] however, then the edit distance at position (i, j) is equal to 1 + the minimum of the edit distances at positions (i - 1, j), (i, j - 1), and (i - 1, j - 1). Why is that the case? </strong></i>
</details>

<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>Do you really need to store the entire two-dimensional array mentioned in Hint #1? Identify what stored values you actually use throughout the process of building the array and come up with a way of storing only what you need and nothing more. </strong></i>
</details>

<br>

## Method 1

```tex
【O(nm)time∣O(n,m)space】
```

```tex
1.\ Where\ n\ and\ m\ are\ the\ length\ of\ the\ two\ inptu\ strings\\
```

```java
package Dynamic;

/**
 * @author zhengstars
 * @date 2024/01/02
 */
public class LevenshteinDistance2 {
    // Define a method to calculate the Levenshtein distance between two strings
    public static int levenshteinDistance(String str1, String str2) {
        // Create a matrix dp, where dp[i][j] represents the Levenshtein distance between the first i characters of str1 and the first j characters of str2
        int[][] dp = new int[str1.length() + 1][str2.length() + 1];

        // Traverse the strings
        for (int i = 0; i < str1.length() + 1; i++) {
            for (int j = 0; j < str2.length() + 1; j++) {

                // When i is 0, it means the first string is empty, the distance is j, that is the length of the second string
                if (i == 0) {
                    dp[i][j] = j;
                }

                // When j is 0, it means the second string is empty, the distance is i, that is the length of the first string
                else if (j == 0) {
                    dp[i][j] = i;
                }

                // When characters are the same, the distance does not change
                else if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }

                // When characters are different, the distance increases by 1, and it becomes the minimum value of the three adjacent cells plus one
                else {
                    dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i][j - 1], dp[i - 1][j])) + 1;
                }
            }
        }

        // The last cell in the dp matrix is the Levenshtein distance between the two strings
        return dp[str1.length()][str2.length()];
    }

    public static void main(String[] args) {
        String str1 = "abc";
        String str2 = "adb";
        System.out.println(levenshteinDistance(str1, str2));
    }
}
```



## Method 2

```tex
【O(nm)time∣O(min(n,m))space】
```

```tex
1.\ Where\ n\ and\ m\ are\ the\ length\ of\ the\ two\ inptu\ strings\\
```

```java
package Dynamic;

/**
 * @author zhengstars
 * @date 2024/01/02
 */
public class LevenshteinDistance {
    public static int levenshteinDistance(String str1, String str2) {
        // Firstly, determine the shorter string and the longer string
        String small = str1.length() < str2.length() ? str1 : str2;
        String big = str1.length() >= str2.length() ? str1 : str2;
        // Initialize two arrays, the length is the length of the shorter string + 1
        int[] evenEdits = new int[small.length() + 1];
        int[] oddEdits = new int[small.length() + 1];
        // Fill the evenEdits array, which stands for the minimum operating number for the first j characters of the small string to become the empty string, i.e., deleting j times
        for (int j = 0; j < small.length() + 1; j++) {
            evenEdits[j] = j;
        }
        // Define two array references, used to express current row and previous row
        int[] currentEdits;
        int[] previousEdits;
        // Start traversing the big string
        for (int i = 1; i < big.length() + 1; i++) {
            // Alternately use evenEdits and oddEdits depending on the parity of i,
            // When i is odd, the current row is oddEdits, and the previous row is evenEdits
            // When i is even, the current row is evenEdits, and the previous row is oddEdits
            if (i % 2 == 1) {
                currentEdits = oddEdits;
                previousEdits = evenEdits;
            } else {
                currentEdits = evenEdits;
                previousEdits = oddEdits;
            }
            // For each row, the first element is always equal to the row number, which is the minimum operation number required for the first i characters of the big string to become empty string, i.e., deleting i times
            currentEdits[0] = i;
            // Start traversing the small string
            for (int j = 1; j < small.length() + 1; j++) {
                // If the current characters are the same, no operation is needed,
                // i.e., the minimum operation number in the upper left corner, i.e., previousEdits[j - 1]
                if (big.charAt(i - 1) == small.charAt(j - 1)) {
                    currentEdits[j] = previousEdits[j - 1];
                } else {
                    // If the characters are different, it is one of the three operations, replace(previousEdits[j - 1]), delete(previousEdits[j]), insert(currentEdits[j - 1])
                    // Take the minimum value after adding 1 to these three numbers
                    currentEdits[j] = 1 + Math.min(previousEdits[j - 1], Math.min(previousEdits[j], currentEdits[j - 1]));
                }
            }
        }
        // Return the result, if the length of the string is even, the last row is an even row, i.e., evenEdits, otherwise it is oddEdits
        return big.length() % 2 == 0 ? evenEdits[small.length()] : oddEdits[small.length()];
    }


    public static void main(String[] args) {
        String str1 = "abc";
        String str2 = "adb";
        System.out.println(levenshteinDistance(str1, str2));
    }
}
```



