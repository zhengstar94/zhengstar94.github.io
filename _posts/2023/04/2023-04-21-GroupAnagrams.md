---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Group Anagrams"
date: "2023-04-21"
categories:
  - "Algorithm String"
---

# Group Anagrams [Medium]

- Write a function that takes in an array of strings and groups anagrams together.
- Anagrams are strings made up of exactly the same letters, where order doesn't matter. For example, "cinema" and "iceman" are anagrams; similarly, "foo" and "ofo" are anagrams.
- Your function should return a list of anagram groups in no particular order.

**Sample Input**

```
words = ["yo", "act", "flop", "tac", "foo", "cat", "oy", "olfp"]
```

**Sample Output**

```
[["yo", "oy"], ["flop", "olfp"], ["act", "tac", "cat"], ["foo"]]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try rearranging every input string such that each string's letters are ordered in alphabetical order. What can you do with the resulting strings?</strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> For any two of the resulting strings mentioned in Hint #1 that are equal to each other, their original strings (with their letters in normal order) must be anagrams. Realizing this, you could bucket all of these resulting strings together, all the while keeping track of their original strings, to find the groups of anagrams. </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Can you simply store the resulting strings mentioned in Hint #1 in a hash table and find the groups of anagrams using this hash table? </strong></i>
</details>



<br>

## Method 1

```tex
【O(n*m*log(m))time∣O(nm)space】
```

```tex
1.\ n\ is\ the\ length\ of\ the\ string\ array\\
2.\ m\ is\ the\ average\ length\ of\ the\ string\\
```

```java
package String;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/05/23
 */
public class GroupAnagrams {
    public static List<List<String>> groupAnagrams(String[] words) {
        // Create a HashMap to store the grouped anagrams
        Map<String, List<String>> anagramGroups = new HashMap<>();

        for (String word : words) {
            // Sort the characters in the word
            char[] chars = word.toCharArray();
            Arrays.sort(chars);
            String sortedWord = new String(chars);

            // Add the original word to the corresponding value list, using the sorted word as the key
            if (!anagramGroups.containsKey(sortedWord)) {
                anagramGroups.put(sortedWord, new ArrayList<>());
            }
            anagramGroups.get(sortedWord).add(word);
        }

        // Convert the values of the HashMap to a List and return the result
        return new ArrayList<>(anagramGroups.values());
    }

    public static void main(String[] args) {
        String[] word = new String[]{"yo", "act", "flop", "tac", "foo", "cat", "oy", "olfp"};
        System.out.println(groupAnagrams(word));
    }
}

```

