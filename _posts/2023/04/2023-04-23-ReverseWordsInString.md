---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Reverse Words In String"
date: "2023-04-23"
categories:
  - "Algorithm String"
---

# Reverse Words In String [Medium]

- Write a function that takes in a string of words separated by one or more whitespaces and returns a string that has these words in reverse order. For example, given the string "tim is great", your function should return "great is tim".
- For this problem, a word can contain special characters, punctuation, and numbers. The words in the string will be separated by one or more whitespaces, and the reversed string must contain the same whitespaces as the original string. For example, given the string "whitespaces 4" you would be expected to return "4 whitespaces".
- Note that you're not allowed to to use any built-in split or reverse methods/functions. However, you are allowed to use a built-in join method/function.
- Also note that the input string isn't guaranteed to always contain words.

**Sample Input**

```
string = "AlgoExpert is the best!"
```

**Sample Output**

```
"best! the is AlgoExpert"
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> There are at least two ways to solve this problem, and both require locating the words in the string. How can you find all of the words in the string? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> If you're able to locate all of the words in the string, the next step is to figure out how many spaces are between them. If you can create a list that contains all of the words in the string and all of the spaces between them, then all you need to do is reverse the list and recreate the string using the reversed list. </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> A potentially easier approach to this problem is to start by reversing the entire string. Once the entire string has been reversed, the words will be in the correct order, but each word will also be reversed. From here, all you have to do is reverse all of the individual words in this new string. By doing this, you'll restore each reversed word back to its original order, and you'll have the desired output. </strong></i>
</details>



<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package String;

import java.util.ArrayList;
import java.util.Collections;

/**
 * @author zhengstars
 * @date 2023/05/27
 */
public class ReverseWordsInString {
    public static String reverseWordsInString(String string) {
        ArrayList<String> words = new ArrayList<String>();
        int startOfWord = 0;
        for (int i = 0; i < string.length(); i++) {
            char character = string.charAt(i);
            // If the current character is a space, a word has ended
            if (character == ' ') {
                // Add the word to the list
                words.add(string.substring(startOfWord, i));
                // Update the start position of the next word
                startOfWord = i; 
            }
            // If the current character is not a space, but the previous character is a space, the current position is the start position of a new word
            else if (string.charAt(startOfWord) == ' ') 
            {
                // Add a space to the list
                words.add(" ");
                // Update the start position of the next word
                startOfWord = i; 
            }
        }

        // Add the last word to the list
        words.add(string.substring(startOfWord));
        // Reverse the order of words in the list
        Collections.reverse(words);
        // Join the words in the list into a string and return it
        return String.join("", words); 
    }

    public static void main(String[] args) {
        System.out.println(reverseWordsInString("AlgoExpert is the best!"));
    }
}
```

