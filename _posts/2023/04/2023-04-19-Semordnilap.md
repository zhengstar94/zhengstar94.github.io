---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Semordnilap"
date: "2023-04-19"
categories:
  - "Algorithm String"
---

# Semordnilap [Easy]

- Write a function that takes in an array of strings as input and returns a list containing pairs of strings that satisfy the following conditions:
  - The pair of strings consists of two strings from the array.
  - The pair of strings satisfies the following conditions:
    - Both strings have the same length.
    - Both strings are semordnilap, meaning one string is the reverse spelling of the other.


**Sample Input**

```
String[] words = {"live", "evil", "act", "cat", "god", "dog"};
```

**Sample Output**

```
[["live", "evil"], ["god", "dog"]]
```

<br>

## Method 1

```tex
【O(n*m)time∣O(n*m)space】
```

```tex
1.\ n\ is\ the\ length\ of\ the\ array\ of\ words\\
2.\ m\ is\ the\ length\ of\ the\ longest\ word\\
```

```java
package String;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengstars
 * @date 2023/05/18
 */
public class Semordnilap {
    public static List<List<String>> findSemordnilapPairs(String[] words) {
        List<List<String>> result = new ArrayList<>();
        // Map to store word and its reverse form
        Map<String, String> wordToReverse = new HashMap<>();
        // Map to track existing pairs
        Map<String, Boolean> pairExists = new HashMap<>();

        for (String word : words) {
            // Reverse the word
            String reverse = new StringBuilder(word).reverse().toString();

            // Check if the reverse form of the word exists in the map and if the pair is not already found
            if (wordToReverse.containsKey(reverse) && !pairExists.containsKey(reverse)) {
                List<String> pair = new ArrayList<>();
                pair.add(word);
                pair.add(reverse);
                result.add(pair);

                // Mark the pair as found
                pairExists.put(word, true);
                pairExists.put(reverse, true);
            }
            // Store the word and its reverse form in the map
            wordToReverse.put(word, reverse);
        }

        return result;
    }

    public static void main(String[] args) {
        String[] words = {"live", "evil", "act", "cat", "god", "dog"};
        System.out.println(findSemordnilapPairs(words));
    }
}
```

