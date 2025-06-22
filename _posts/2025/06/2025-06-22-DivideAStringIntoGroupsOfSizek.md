---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2138. Divide a String Into Groups of Size k"
date: "2025-06-22"
tags: Easy
categories:
  - "LeetCode String"
---

- A string `s` can be partitioned into groups of size `k` using the following procedure:
  - The first group consists of the first `k` characters of the string, the second group consists of the next `k` characters of the string, and so on. Each element can be a part of **exactly one** group.
  - For the last group, if the string **does not** have `k` characters remaining, a character `fill` is used to complete the group.
- Note that the partition is done so that after removing the `fill` character from the last group (if it exists) and concatenating all the groups in order, the resultant string should be `s`.
- Given the string `s`, the size of each group `k` and the character `fill`, return *a string array denoting the **composition of every group*** `s` *has been divided into, using the above procedure*.

**Example 1**

```
Input: s = "abcdefghi", k = 3, fill = "x"
Output: ["abc","def","ghi"]
Explanation:
The first 3 characters "abc" form the first group.
The next 3 characters "def" form the second group.
The last 3 characters "ghi" form the third group.
Since all groups can be completely filled by characters from the string, we do not need to use fill.
Thus, the groups formed are "abc", "def", and "ghi".
```

**Example 2**

```
Input: s = "abcdefghij", k = 3, fill = "x"
Output: ["abc","def","ghi","jxx"]
Explanation:
Similar to the previous example, we are forming the first three groups "abc", "def", and "ghi".
For the last group, we can only use the character 'j' from the string. To complete this group, we add 'x' twice.
Thus, the 4 groups formed are "abc", "def", "ghi", and "jxx".
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.String;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/06/22
 */
public class DivideAStringIntoGroupsOfSizek {

    public static String[] divideString(String s, int k, char fill) {
        int n = s.length(); // Get the length of the input string.

        // Calculate the total number of groups needed.
        // (n + k - 1) / k is a common trick to perform integer division with rounding up.
        // For example, if n=10 and k=3, (10+3-1)/3 = 12/3 = 4 groups.
        // This ensures that if there are leftover characters, we still allocate a group for them.
        int groupCount = (n + k - 1) / k;

        // Create an array to store the resulting groups.
        String[] result = new String[groupCount];

        // Iterate over each group index.
        for (int i = 0; i < groupCount; i++) {
            // Calculate the starting index of the current group in the original string.
            int start = i * k;

            // Calculate the ending index (exclusive) for the current group.
            // Use Math.min to avoid going out of bounds if the last group is shorter than k.
            int end = Math.min(start + k, n);

            // Use StringBuilder for efficient string concatenation.
            StringBuilder sb = new StringBuilder();

            // Append the substring for the current group.
            // This will add all available characters from start to end (end not included).
            sb.append(s, start, end);

            // If the current group is shorter than k, pad it with the fill character.
            // This loop will add as many fill characters as needed to reach length k.
            while (sb.length() < k) {
                sb.append(fill);
            }

            // Convert the StringBuilder to a String and store it in the result array.
            result[i] = sb.toString();
        }

        // Return the array containing all the groups.
        return result;
    }

    // Main method with test cases to demonstrate the function.
    public static void main(String[] args) {
        // Test case 1: String length is a multiple of k, no padding needed.
        System.out.println(Arrays.toString(divideString("abcdefghi", 3, 'x') ) ); // Output: ["abc", "def", "ghi"]

        // Test case 2: String length is not a multiple of k, last group needs padding.
        System.out.println(Arrays.toString(divideString("abcdefghij", 3, 'x') ) ); // Output: ["abc", "def", "ghi", "jxx"]
    }
}
```





