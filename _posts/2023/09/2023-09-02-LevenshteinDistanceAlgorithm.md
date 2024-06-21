---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Levenshtein Distance Algorithm"
date: "2023-09-02"
categories: 
  - "Data Structure"
---

# Levenshtein Distance Algorithm

Levenshtein Distance, also known as edit distance, is used to measure the difference between two strings. By taking the minimum number of steps to convert one string to another, this step number represents the "distance" between the two strings. Specifically, steps can be inserting a new character, deleting an existing character, or replacing a character.

## Algorithm description

The core of the edit distance algorithm is the dynamic programming algorithm, with detailed steps as follows:

1. We use a two-dimensional array `dp[i][j]` to represent the shortest edit distance from string s of length i to string t of length j.
2. When `i=0`, it means s is an empty string, so it needs to insert j characters to change to t, so `dp[i][j] = j`.
3. When `j=0`, it means t is an empty string, so it needs to delete i characters to change from s to t, so `dp[i][j] = i`.
4. When `i!=0` and `j!=0`, we need to consider whether `s[i-1]` and `t[j-1]` are equal, if they are equal, then `dp[i][j] = dp[i-1][j-1]`; if they are not equal, then we need to consider the minimum of the following three situations:
  1. Replace `s[i-1]`, that is, let `s[i-1]` become `t[j-1]`, so `dp[i][j] = 1 + dp[i-1][j-1]`.
  2. Delete `s[i-1]`, let `s[0...i-2]` become `t[0...j-1]`, that is, `dp[i][j] = 1 + dp[i-1][j]`.
  3. Insert `t[j-1]` into s, let `s[0...i-1]` become `t[0...j-2]`, that is, `dp[i][j] = 1 + dp[i][j-1]`.



## Algorithm Implementation 1

Below is a Java implementation of the Levenshtein Distance algorithm:

```java
/**【O(nm)time∣O(nm)space】**/
public class LevenshteinDistance {
    // Define the method for calculating the Levenshtein distance between two strings
    public static int levenshteinDistance(String str1, String str2) {
        // Create a matrix dp where dp[i][j] represents the Levenshtein distance between the first i characters of str1 and the first j characters of str2
        int[][] dp = new int[str1.length() + 1][str2.length() + 1];

        // Iterate over the strings
        for (int i = 0; i < str1.length() + 1; i++) {
            for (int j = 0; j < str2.length() + 1; j++) {

                // When i is 0, meaning that the first string is empty, the distance is j, which is the length of the second string
                if (i == 0) {
                    dp[i][j] = j;
                } 

                // When j is 0, meaning that the second string is empty, the distance is i, which is the length of the first string
                else if (j == 0) {
                    dp[i][j] = i;
                } 

                // When the characters are the same, the distance doesn't change
                else if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }

                // When the characters are different, the distance increases by 1 and it becomes the minimum value of the three neighboring cells plus one
                else {
                    dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i][j - 1], dp[i - 1][j])) + 1;
                }
            }
        }

        // The last cell of the matrix dp is the Levenshtein distance between the two strings
        return dp[str1.length()][str2.length()];
    }
}
```



### Detailed Explanation 1

First, we construct a two-dimensional array dp where dp[i][j] represents the Levenshtein Distance between the first i characters of string str1 and the first j characters of string str2.

Then, we traverse the two-dimensional array through a double loop and calculate the value of dp\[i][j]. The value of dp[i][j] may have the following situations:

1. If str1 or str2 is an empty string (length is 0), then dp[i][j] is the length of the other string because we need at least i or j insert operations to match the two strings.
2. If the i-th character of str1 and the j-th character of str2 are the same, then dp\[i][j] is dp\[i-1][j-1], because we do not need any additional operations from str1 to str2.
3. If the i-th character of str1 and the j-th character of str2 are different, then dp[i][j] is the minimum value plus 1 before dp\[i-1][j-1] (replacement operation), dp\[i][j-1] (insert operation) and dp\[i-1][j] (delete operation).

Finally, return dp\[str1.length][str2.length] as the result, which represents the minimum number of operations required to fully convert string str1 to string str2.

## Algorithm Implementation 2

```java
/**【O(nm)time∣O(min(n,m))space】 **/
public class LevenshteinDistance {
    // Method for calculating Levenshtein Distance
    public static int levenshteinDistance(String str1, String str2) {
        // Identify which string is smaller and which is bigger
        String small = str1.length() < str2.length() ? str1 : str2;
        String big = str1.length() >= str2.length() ? str1 : str2;

        // Create two arrays to keep track of the current and previous edit distances
        int[] evenEdits = new int[small.length() + 1];
        int[] oddEdits = new int[small.length() + 1];

        // Initialize the first row in evenEdits array
        for (int j = 0; j < small.length() + 1; j++) {
            evenEdits[j] = j;
        }

        // Variables to reference the current and previous edit distances
        int[] currentEdits;
        int[] previousEdits;

        // Loop over characters in the big string
        for (int i = 1; i < big.length() + 1; i++) {
            // Determine whether the current row is even or odd
            if (i % 2 == 1) {
                currentEdits = oddEdits;
                previousEdits = evenEdits;
            } else {
                currentEdits = evenEdits;
                previousEdits = oddEdits;
            }

            // The first cell of the current row is just the row number i
            currentEdits[0] = i;

            // Loop over the characters in the small string
            for (int j = 1; j < small.length() + 1; j++) {
                // If the characters in the current cell are the same
                if (big.charAt(i - 1) == small.charAt(j - 1)) {
                    // The edit distance is simply the diagonal
                    currentEdits[j] = previousEdits[j - 1];
                } else {
                    // The edit distance is the smallest of three options:
                    // insertion, deletion, and substitution
                    currentEdits[j] = 1 + Math.min(previousEdits[j - 1], Math.min(previousEdits[j], currentEdits[j - 1]));
                }
            }
        }

        // Return the last cell from the most recent computed row
        return big.length() % 2 == 0 ? evenEdits[small.length()] : oddEdits[small.length()];
    }
}
```

### Detailed Explanation 2

While the optimized code may seem complex, its underlying concept is still dynamic programming, building the solution step by step to get the final answer, only making adjustments in the details to reduce space consumption.

#### **1. Determine Smaller and Larger Strings**

```java
String small = str1.length() < str2.length() ? str1 : str2;
String big = str1.length() >= str2.length() ? str1 : str2;
```

#### **2. Create Two Arrays evenEdits and oddEdits**

```java
int[] evenEdits = new int[small.length() + 1];
int[] oddEdits = new int[small.length() + 1];
```

evenEdits and oddEdits are used to store the edit distances of even and odd rows respectively. The size is related to the length of the smaller string. As we only care about the edit distance of the previous row and the current row, two arrays are used to save space.

#### **3. Initialise evenEdits Array**

```java
for (int j = 0; j < small.length() + 1; j++) {
    evenEdits[j] = j;
}
```

#### **4. Start Double Loops**

```java
for (int i = 1; i < big.length() + 1; i++) {
    ...
    for (int j = 1; j < small.length() + 1; j++) {
        ...
    }
}
```

For each row (each character of big in this case), we calculate the edit distance between it and each character of small. When i is even, we use evenEdits array to store the result; when i is odd, we use oddEdits array.

#### **5. Calculate Edit Distance**

```java
if (big.charAt(i - 1) == small.charAt(j - 1)) {
    currentEdits[j] = previousEdits[j - 1];
} else {
    currentEdits[j] = 1 + Math.min(previousEdits[j - 1], Math.min(previousEdits[j], currentEdits[j - 1]));
}
```

Here we decide whether to keep the value of previousEdits[j - 1] (value of the diagonal) or to set it to 1 + the previous minimum edit distance, based on whether the current character of big and small are equal. The smaller value comes from one of three sources: previousEdits[j - 1] (represents replacement operation), previousEdits[j] (represents delete operation), or currentEdits[j - 1] (represents insert operation).

#### **6. Return Result**

```java
return big.length() % 2 == 0 ? evenEdits[small.length()] : oddEdits[small.length()];
```

Finally, the edit distance is in the last element of the last row. Depending on whether the length of big is odd or even, we decide to return evenEdits[small.length()] or oddEdits[small.length()]).

In this way, we optimized the original space complexity from O(m*n) to O(min(m,n)).

## Algorithm Application

The Levenshtein Distance is widely used in natural language processing and data processing, including word spelling checking, speech recognition, fuzzy string queries, and bioinformatics (such as DNA string matching).

Above is a detailed explanation and implementation of the Levenshtein Distance algorithm. I hope this helps you understand it. Although this algorithm is concise, it is very practical. The ideas of dynamic programming and space optimization techniques in the evolution process are the important reasons for the algorithm's classic status and are worth pondering.

