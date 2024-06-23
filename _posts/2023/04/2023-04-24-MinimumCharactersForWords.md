---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Minimum Characters For Words"
date: "2023-04-24"
categories:
  - "Algorithm String"
---

# Minimum Characters For Words [Medium]

- Write a function that takes in an array of words and returns the smallest array of characters needed to form all of the words. The characters don't need to be in any particular order.
- For example, the characters ["y", "r", "o", "u"] are needed to form the words ["your", "you", "or", "yo"].
- Note: the input words won't contain any spaces; however, they might contain punctuation and/or special characters.

**Sample Input**

```
words = ["this", "that", "did", "deed", "them!", "a"]
```

**Sample Output**

```
["t", "t", "h", "i", "s", "a", "d", "d", "e", "e", "m", "!"]
// The characters could be ordered differently.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> There are a few different ways to solve this problem, but all of them use the same general approach. You'll need to determine not only all of the unique characters required to form the input words, but also their required frequencies. What determines the required frequencies of characters to form multiple words? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The word that contains the highest frequency of any character dictates how many of those characters are required. For example, given words = ["A", "AAAA"] you need 4 As, because the word that contains the most of amount of As has 4. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Use a hash table to keep track of the maximum frequencies of all unique characters that occur across all words. Count the frequency of each character in each word, and use those per-word frequencies to update your maximum-character-frequency hash table. Once you've determined the maximum frequency of each character across all words, you can use the built-up hash table to generate your output array. </strong></i>
</details>



<br>

## Method 1

```tex
【O(n*l)time∣O(c)space】
```

```tex
1.\ n\ is\ the\ length\ of\ words\\
2.\ l\ is\ the\ length\ of\ the\ longest\ word\\
3.\ c\ is\ the\ number\ of\ unique\ characters\ across\ all\ words\\
```

```java
package String;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengstars
 * @date 2023/05/27
 */
public class MinimumCharactersForWords {
    public static char[] minimumCharactersForWords(String[] words) {
        // Used to store the maximum frequency of each character
        Map<Character, Integer> maxCharFrequencies = new HashMap<>();

        // Iterate through each word
        for (String word : words) {
            // Used to store the frequency of each character in the current word
            Map<Character, Integer> charFrequencies = new HashMap<>();
            // Iterate through each character in the current word
            for (int i = 0; i < word.length(); i++) {
                // Get the current character
                char character = word.charAt(i);
                // Increment the frequency of the current character by one
                charFrequencies.put(character, charFrequencies.getOrDefault(character, 0) + 1);
            }

            for (Map.Entry<Character, Integer> entry : charFrequencies.entrySet()) {
                // Get the current character
                char character = entry.getKey();
                // Get the frequency of the current character
                int frequency = entry.getValue();
                // Update the maximum frequency of each character
                maxCharFrequencies.put(character, Math.max(maxCharFrequencies.getOrDefault(character, 0), frequency));
            }
        }

        // Store the result characters in a list
        List<Character> result = new ArrayList<>();
        for (Map.Entry<Character, Integer> entry : maxCharFrequencies.entrySet()) {
            // Get the character
            char character = entry.getKey();
            // Get the maximum frequency of the character
            int frequency = entry.getValue();
            // Add the character to the result list based on its maximum frequency
            for (int i = 0; i < frequency; i++) {
                result.add(character);
            }
        }

        // Convert the result list to a character array
        char[] resultArray = new char[result.size()];
        for (int i = 0; i < result.size(); i++) {
            resultArray[i] = result.get(i);
        }

        // Return the result character array
        return resultArray;
    }

    public static void main(String[] args) {
        String[] words = new String[]{"this", "that", "did", "deed", "them!", "a"};
        System.out.println(minimumCharactersForWords(words));
    }
}

```

