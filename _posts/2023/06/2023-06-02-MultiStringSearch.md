---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Multi String Search"
date: "2023-06-02"
categories:
  - "Algorithm Tries"
---

# Multi String Search [Hard]

- Write a function that takes in a big string and an array of small strings, all of which are smaller in length than the big string. The function should return an array of booleans, where each boolean represents whether the small string at that index in the array of small strings is contained in the big string.
- Note that you can't use language-built-in string-matching methods.

**Sample Input 1**


```
bigString = "this is a big string"
smallStrings = ["this", "yo", "is", "a", "bigger", "string", "kappa"]
```

**Sample Output 1**

```
[true, false, true, true, false, true, false]
```



**Sample Input 1**


```
bigString = "abcdefghijklmnopqrstuvwxyz"
smallStrings = ["abc", "mnopqr", "wyz", "no", "e", "tuuv"]
```

**Sample Output 2**

```
[true, true, false, true, true, false]
```





**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> A simple way to solve this problem is to iterate through all of the small strings, checking if each of them is contained in the big string by iterating through the big string's characters and comparing them to the given small string's characters with a couple of loops. Is this approach efficient from a time-complexity point of view?</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Try building a suffix-trie-like data structure containing all of the big string's suffixes. Then, iterate through all of the small strings and check if each of them is contained in the data structure you've created. What are the time-complexity ramifications of this approach? </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Try building a trie containing all of the small strings. Then, iterate through the big string's characters and check if any part of the big string is a string contained in the trie you've created. Is this approach better than the one described in Hint #2 from a time-complexity point of view?</strong></i>
</details>


<br>



## Method 1

```tex
【O(ns + bs)time∣O(ns)space】
Where ns is the total number of characters in all small strings, and Bs is the total number of characters in a large string.
```

```java
package Tries;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengstars
 * @date 2023/09/29
 */
public class MultiStringSearch {

    private static boolean doesContain(String bigString, Trie trie, String smallString) {
        TrieNode currentNode = trie.root;

        // Match each character in the pattern string
        for (char c : smallString.toCharArray()) {
            if (!currentNode.children.containsKey(c)) {
                return false;
            }
            currentNode = currentNode.children.get(c);
        }

        // Length of bigString
        int m = bigString.length();
        // Length of smallString
        int n = smallString.length();
        // i for iterating over bigString
        int i = 0;
        // j for iterating over smallString
        int j = 0;

        // Match the pattern string in bigString
        while (i < m) {
            char bigChar = bigString.charAt(i);

            if (j < n && bigChar == smallString.charAt(j)) {
                // If the current character matches, move forward on bigString
                i++;
                // If the current character matches, move forward on smallString
                j++;
            } else {
                if (j == 0) {
                    // If no match and j is 0, move i
                    i++;
                } else {
                    // Transition to the next state based on the Aho-Corasick algorithm

                    // 1.1 currentNode != null checks if the current state currentNode is not null. If it's null, it means the current character cannot match any pattern string, and state transition is not possible.
                    // 1.2 currentNode.children.containsKey(bigChar) checks if the current state has a child state with bigChar as the character. If it does, it indicates that state transition is possible based on the current character.

                    // 2.1 j = ... ? 1 : 0; decides whether to perform a state transition based on the above two conditions. If the transition is possible, set j to 1, indicating a transition to the next state. Otherwise, set it to 0, indicating no state transition.
                    // 2.2 currentNode = ... updates the current state based on the above conditions. If the transition is possible, update currentNode to the next state's node. Otherwise, keep it as null, indicating no state transition.

                    j = currentNode != null && currentNode.children.containsKey(bigChar) ? 1 : 0;
                    currentNode = currentNode != null ? currentNode.children.get(bigChar) : null;
                }
            }

            if (j == n) {
                // Found a complete match of the pattern string
                return true;
            }
        }

        // Pattern string not found
        return false;
    }

    /**
     * Function for multi-pattern string search
     * @param bigString
     * @param smallStrings
     * @return
     */
    public static List<Boolean> multiStringSearch(String bigString, String[] smallStrings) {
        Trie trie = new Trie();
        for (String smallString : smallStrings) {
            trie.insert(smallString);
        }

        List<Boolean> result = new ArrayList<>();

        // Check if each pattern string exists in bigString
        for (String smallString : smallStrings) {
            result.add(doesContain(bigString, trie, smallString));
        }

        return result;
    }

    public static void main(String[] args) {
        String bigString1 = "this is a big string";
        String[] smallStrings1 = {"this", "yo", "is", "a", "bigger", "string", "stang"};
        List<Boolean> result1 = multiStringSearch(bigString1, smallStrings1);
        System.out.println("Sample Output 1: " + result1);

        String bigString2 = "abcdefghijklmnopqrstuvwxyz";
        String[] smallStrings2 = {"abc", "mnopqr", "wyz", "no", "e", "tuuv"};
        List<Boolean> result2 = multiStringSearch(bigString2, smallStrings2);
        System.out.println("Sample Output 2: " + result2);
    }
}

/**
 * Trie tree node
 */
class TrieNode {
    /**
     * Store child nodes
     */
    Map<Character, TrieNode> children;
    /**
     * Flag to indicate if it's the end of a word
     */
    boolean isEnd;
    /**
     * Store the pattern string
     */
    String word;

    /**
     * Trie tree node constructor
     */
    TrieNode() {
        children = new HashMap<>();
        isEnd = false;
        word = null;
    }
}

/**
 * Trie tree
 */
class Trie {
    TrieNode root;

    // Trie tree constructor
    Trie() {
        root = new TrieNode();
    }

    // Insert a pattern string
    void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            // If the child node does not exist, add it
            node.children.putIfAbsent(c, new TrieNode());
            node = node.children.get(c);
        }
        // Mark the end of the pattern string
        node.isEnd = true;
        // Store the pattern string
        node.word = word;
    }
}

```

