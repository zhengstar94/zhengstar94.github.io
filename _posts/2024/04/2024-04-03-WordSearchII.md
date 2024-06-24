---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "212.Word Search II"
date: "2024-04-03"
categories:
  - "LeetCode Tries"
---

# 212. Word Search II

- Given an `m x n` `board` of characters and a list of strings `words`, return *all words on the board*.
- Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1**

```
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
```

**Example 2**

```
Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []
```

## Method 1

```tex
【O(n * L + m * n * 4^L) time | O(n * L) space】\\
n\ is\ the\ number\ of\ words,\ L\ is\ the\ maximum\ length\ of\ a\ word,\ m\ is\ the\ number\ of\ rows\ in\ the\ board,\ n\ is\ the\ number\ of\ columns\ in\ the\ board\\

```

```java
package Leetcode.Tries;

/**
 * @author zhengstars
 * @date 2024/06/01
 */
class TriesNode {
    boolean isEnd;
    TriesNode[] next;

    /**
     * Constructor to initialize a Trie node.
     * isEnd indicates if the node is the end of a word.
     * next is an array representing the 26 possible children (for each letter a-z).
     */
    public TriesNode() {
        isEnd = false;
        next = new TriesNode[26];
    }
}

public class WordDictionary {
    TriesNode root;

    /**
     * Constructor to initialize the WordDictionary with a root Trie node.
     */
    public WordDictionary() {
        root = new TriesNode();
    }

    /**
     * Adds a word into the WordDictionary.
     * @param word The word to add into the Trie.
     */
    public void addWord(String word) {
        TriesNode node = root;
        // Convert the word to a character array for traversal.
        for (char c : word.toCharArray()) {
            // Calculate the index corresponding to the character 'c'.
            int index = c - 'a';
            // If the next node at this index is null, create a new Trie node.
            if (node.next[index] == null) {
                node.next[index] = new TriesNode();
            }
            // Move to the next node.
            node = node.next[index];
        }
        // Mark the end of the word.
        node.isEnd = true;
    }

    /**
     * Searches for a word in the WordDictionary, which may contain the '.' wildcard that can match any letter.
     * @param word The word to search, potentially containing the wildcard character '.'.
     * @return True if the word is in the WordDictionary, otherwise false.
     */
    public boolean search(String word) {
        // Start the search from the root node and initial index 0.
        return search(root, word, 0);
    }

    /**
     * Helper method for search, called recursively.
     * @param node The current Trie node being checked.
     * @param word The word being searched.
     * @param idx The current index within the word being checked.
     * @return True if the word matches in the Trie, otherwise false.
     */
    private boolean search(TriesNode node, String word, int idx) {
        // If the current index is equal to the word's length, return whether this node marks the end of a word.
        if (idx == word.length()) {
            return node.isEnd;
        }
        char c = word.charAt(idx);
        // If the current character is '.', it can match any letter.
        if (c == '.') {
            // Check all possible children.
            for (int i = 0; i < 26; i++) {
                if (node.next[i] != null && search(node.next[i], word, idx + 1)) {
                    return true;
                }
            }
            return false;
        } else {
            // If it's a specific character, check the corresponding child node.
            int index = c - 'a';
            if (node.next[index] == null) {
                return false;
            }
            // Continue the search in the next node.
            return search(node.next[index], word, idx + 1);
        }
    }

    public static void main(String[] args) {
        WordDictionary wordDictionary = new WordDictionary();
        wordDictionary.addWord("bad");
        wordDictionary.addWord("dad");
        wordDictionary.addWord("mad");

        // Test cases
        //System.out.println(wordDictionary.search("pad")); // false, "pad" is not present.
        //System.out.println(wordDictionary.search("bad")); // true, "bad" is present.
        System.out.println(wordDictionary.search(".ad")); // true, matches "bad", "dad", "mad".
        System.out.println(wordDictionary.search("b..")); // true, matches "bad".

        // Additional test cases
        wordDictionary.addWord("cat");
        wordDictionary.addWord("bat");
        wordDictionary.addWord("rat");
        System.out.println(wordDictionary.search("...")); // true, matches any three-letter word.
        System.out.println(wordDictionary.search("cats")); // false, does not match any word.
        System.out.println(wordDictionary.search("c.t")); // true, matches "cat".

        // Edge case: Empty string
        System.out.println(wordDictionary.search("")); // false, assuming empty strings are not valid words.
    }
}



```

