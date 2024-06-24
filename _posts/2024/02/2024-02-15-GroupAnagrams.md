---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "49.Group Anagrams"
date: "2024-02-15"
categories:
  - "LeetCode HashTable"
---

# LeetCode 49. Group Anagrams

- Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.
- An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2**

```
Input: strs = [""]
Output: [[""]]
```

**Example 3**

```
Input: strs = ["a"]
Output: [["a"]]
```

## Method 1

```tex
【O(nmlog(m))time∣O(nm)space】
```

```java
package Leetcode.HashTable;

import java.util.*;

/**
 * @author zhengstars
 * @date 2024/02/18
 *
 */
public class GroupAnagrams {

    public static List<List<String>> groupAnagrams(String[] strs) {
        // Instantiate a hash map to hold the anagram groups
        Map<String, List<String>> map = new HashMap<>();

        // Iterate over the input strings
        for (String s : strs) {

            // Convert the string to a character array for sorting
            char[] characters = s.toCharArray();

            // Sort the characters in alphabetical order
            Arrays.sort(characters);

            // Convert the sorted character array back to a String
            String sorted = new String(characters);

            // If the map does not contain the sorted string as a key, add it with a new ArrayList as its value
            if (!map.containsKey(sorted)) {
                map.put(sorted, new ArrayList<>());
            }

            // Add the original (unsorted) string to the anagram group associated with the sorted string key
            map.get(sorted).add(s);
        }

        // Convert the values view of the map (which is a Collection of Lists), to a List of Lists and return as result
        return new ArrayList<>(map.values());
    }

    /**
     * The main function to test the solution.
     *
     * @param args command line arguments
     */
    public static void main(String[] args) {

        // Test case: an array of anagrams
        String[] strs = {"eat","tea","tan","ate","nat","bat"};

        // Call the solution function and print the result
        System.out.println(groupAnagrams(strs)); // Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
    }
}
```

## Method 2

```tex
【O(nm)time∣O(nm)space】
```

```java
package Leetcode.HashTable;

import java.util.*;

/**
 * @author zhengstars
 * @date 2024/02/18
 *
 */
public class GroupAnagrams {

    public static List<List<String>> groupAnagrams(String[] strs) {
        // Instantiate a hash map to hold the groups of anagrams, with the sorted string as the key,
        // and the list of original strings as the value
        Map<String, List<String>> map = new HashMap<>();

        // Iterate over the input array of strings
        for (String str : strs) {
            // Create an array of 26 ints to store the count of each letter in str
            int[] counts = new int[26];

            // Calculate the count of each letter
            for (char c : str.toCharArray()) {
                counts[c - 'a']++;
            }

            // Build a special pattern string with each letter in alphabetical order followed by its count  
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 26; i++) {
                if (counts[i] > 0) {
                    sb.append((char)(i + 'a'));
                    sb.append(counts[i]);
                }
            }
            // Use the special pattern string as the key for the hash map
            String key = sb.toString();

            // Add the original string to the anagram list associated with the current key
            // If the key is not already in the hash map, add it with an empty list as the value
            map.putIfAbsent(key, new ArrayList<>());
            map.get(key).add(str);
        }

        // Return a list of all of the anagram group lists
        return new ArrayList<>(map.values());
    }

    public static void main(String[] args) {
        // Sample input array of strings
        String[] strs = {"eat","tea","tan","ate","nat","bat"};

        // Display the list of anagrams groups to the console
        System.out.println(groupAnagrams(strs)); // Output: [["bat"],["nat", "tan"],["eat", "tea", "ate"]]
    }
}
```
