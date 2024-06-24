---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Suffix Trie Construction"
date: "2023-06-01"
categories:
  - "Algorithm Tries"
---

# Suffix Trie Construction [Medium]

- Write a SuffixTrie class for a Suffix-Trie-like data structure. The class should have a root property set to be the root node of the trie and should support:
- Creating the trie from a string; this will be done by calling the populateSuffixTrieFrom method upon class instantiation, which should populate the root of the class. Searching for strings in the trie. Note that every string added to the trie should end with the special endSymbol character: "*".
- If you're unfamiliar with Suffix Tries, we recommend watching the Conceptual Overview section of this question's video explanation before starting to code.

**Sample Input (for creation)**


```
string = "babc"
```

**Sample Output (for creation)**

```
The structure below is the root of the trie.
{
  "c": {"*": true},
  "b": {
    "c": {"*": true},
    "a": {"b": {"c": {"*": true}}},
  },
  "a": {"b": {"c": {"*": true}}},
}
```



**Sample Input (for searching in the suffix trie above)**


```
string = "abc"
```

**Sample Output (for searching in the suffix trie above)**

```
true
```





**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Building a suffix-trie-like data structure consists of essentially storing every suffix of a given string in a trie. To do so, iterate through the input string one character at a time and insert every substring starting at each character and ending at the end of the string into the trie.</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> To insert a string into the trie, start by adding the first character of the string into the root node of the trie and mapping it to an empty hash table if it isn't already there. Then, iterate through the rest of the string inserting each of the remaining characters into the previous character's corresponding node (or hash table) in the trie, making sure to add an endSymbol "*" at the end. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Searching the trie for a specific string should follow a nearly identical logic to the one used to add a string in the trie.</strong></i>
</details>


<br>



## Method 1

```tex
Creation:【O(n^2)time∣O(n^2)space】
where n is the length of the input string 
```

```tex
Searching:【O(m)time∣O(1)space】
where m is the length of the input string
```

```java
package Tries;

import java.util.HashMap;
import java.util.Map;

public class SuffixTrie {
    /**
     * A special symbol that indicates the end of a word.
     */
    private static final char END_SYMBOL = '*';
    /**
     * Store the mapping of child nodes
     */
    private Map<Character, SuffixTrie> children;
    /**
     * if the mark is the end of the word
     */
    private boolean isEnd;

    /**
     * Constructor, initialize child node mapping and whether it is the end of a word
     */
    public SuffixTrie() {
        this.children = new HashMap<>();
        this.isEnd = false;
    }

    /**
     * Insert a suffix into the suffix dictionary tree
     * @param suffix
     */
    public void insertSuffix(String suffix) {
        SuffixTrie node = this;
        for (char c : suffix.toCharArray()) {
            // If the character is not in the child node mapping, a new child node is added
            node.children.putIfAbsent(c, new SuffixTrie());
            node = node.children.get(c);
        }
        // End of suffix, add end symbol
        node.children.put(END_SYMBOL, null);
        // Mark as the end of a word
        node.isEnd = true;
    }

    /**
     * Search for whether the string exists in the suffix dictionary tree
     * @param str
     * @return
     */
    public boolean contains(String str) {
        SuffixTrie node = this;
        for (char c : str.toCharArray()) {
            if (!node.children.containsKey(c)) {
                // The current character is not included in the child node mapping, and the string does not exist
                return false;
            }
            node = node.children.get(c);
        }
        // 返回是否为单词结尾
        return node.isEnd;
    }
}

class Main {
    public static void main(String[] args) {
        String inputString = "babc";
        SuffixTrie suffixTrie = new SuffixTrie();

        // Insert all suffixes into the suffix dictionary tree
        for (int i = 0; i < inputString.length(); i++) {
            suffixTrie.insertSuffix(inputString.substring(i));
        }

        String searchString = "abc";
        boolean contains = suffixTrie.contains(searchString);
        System.out.println(contains);
    }
}
```

