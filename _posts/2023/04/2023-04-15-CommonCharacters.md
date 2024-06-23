---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Common Characters"
date: "2023-04-15"
categories:
  - "Algorithm String"
---


# Common Characters [Easy]

- Given a string array words, return an array of all characters that show up in all strings within the words (including duplicates). You may return the answer in any order.

**Sample Input**

```
words = ["cool","lock","cook"]
```

**Sample Output**

```
["c","o"]
```



## Method 1

```tex
【O(nm)time∣O(1)space】
```

```java
package String;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/05/14
 */
public class CommonCharacters {
    public static List<String> commonChars(String[] A) {
        // Initialize an array of counts of each letter in the first word
        int[] last = count(A[0]);

        // Loop through each word in the array
        for (int i = 1; i < A.length; i++) {
            // Calculate the counts of each letter in the current word
            last = intersection(last, count(A[i]));
        }

        // Create a list to store the common characters
        List<String> arr = new ArrayList<>();

        // Loop through the counts of each letter
        for (int i = 0; i < 26; i++) {
            // If the count of this letter is not zero
            if (last[i] != 0) {
                // Convert the index to the corresponding letter
                char a = 'a';
                a += i;
                String s = String.valueOf(a);
                // Add the letter to the list the number of times it appears
                while (last[i] > 0) {
                    arr.add(s);
                    last[i]--;
                }
            }
        }

        return arr;
    }

    // Calculate the intersection of the counts of two words
    public static int[] intersection(int[] a, int[] b) {
        int[] t = new int[26];
        for (int i = 0; i < 26; i++) {
            t[i] = Math.min(a[i], b[i]);
        }
        return t;
    }

    // Calculate the count of each letter in a word
    public static int[] count(String str) {
        int[] t = new int[26];
        for (char c : str.toCharArray()) {
            t[c - 'a']++;
        }
        return t;
    }

    public static void main(String[] args) {
        String[] A = new String[]{"cool","lock","cook"};
        System.out.println(commonChars(A));
    }
}
```

