---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "208.Implement Trie(Prefix Tree) "
date: "2024-04-01"
categories:
  - "LeetCode Tries"
---

# 208. Implement Trie (Prefix Tree) 

- A [**trie**](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

  Implement the Trie class:

  - `Trie()` Initializes the trie object.
  - `void insert(String word)` Inserts the string `word` into the trie.
  - `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
  - `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

**Example 1**

```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Tries;

/**
 * TrieNode class serves as the building block for the Trie data structure.
 */
class TrieNode {
    public TrieNode[] children;
    public boolean isWord;
    public TrieNode() {
        children = new TrieNode[26];
        isWord = false;
    }
}

/**
 * @author zhengstars
 * @date 2024/05/14
 *
 * The main class `Trie` represents the Trie data structure.
 */
public class Trie {

    /**
     * The root node of the Trie
     */
    private TrieNode root;

    /**
     * Trie constructor that initializes the data structure.
     */
    public Trie() {
        root = new TrieNode();
    }

    /**
     * Inserts a word into the Trie.
     *
     * @param word the word to be inserted
     */
    public void insert(String word) {
        TrieNode node = root;

        // iterate over each character in the word
        for(char c : word.toCharArray()){

            // if the child node for this character doesn't exist
            if(node.children[c - 'a'] == null){
                // create a new node for this character
                node.children[c - 'a'] = new TrieNode();
            }

            // move to the child node
            node = node.children[c - 'a'];
        }

        // mark the end of a word
        node.isWord = true;
    }

    /**
     * Returns true if the word is in the Trie, false otherwise.
     *
     * @param word word to be searched in the Trie
     * @return true if the word is present, false otherwise
     */
    public boolean search(String word) {
        TrieNode node = root;

        // iterate over each character in the word
        for(char c : word.toCharArray()){

            // if the child node for this character doesn't exist
            if(node.children[c - 'a'] == null){
                // the word doesn't exist in the Trie
                return false;
            }
            // move to the child node
            node = node.children[c - 'a'];
        }

        // if the node is marked as a word, then the word exists in the Trie
        return node.isWord;
    }

    /**
     * Returns true if there is any word in the Trie that starts with the given prefix, false otherwise.
     *
     * @param prefix prefix to be searched in the Trie
     * @return true if there's any word that starts with the prefix, false otherwise
     */
    public boolean startsWith(String prefix) {
        TrieNode node = root;

        // iterate over each character in the prefix
        for(char c : prefix.toCharArray()){

            // if the child node for this character doesn't exist
            if(node.children[c - 'a'] == null){
                // no word in the Trie starts with this prefix
                return false;
            }

            // move to the child node
            node = node.children[c - 'a'];
        }

        // if we have reached here, then there is at least one word that starts with this prefix
        return true;
    }
}
```

