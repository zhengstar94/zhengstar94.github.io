---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Generate Document"
date: "2023-04-17"
categories:
  - "Algorithm String"
---

# Generate Document [Easy]

- You're given a string of available characters and a string representing a document that you need to generate. Write a function that determines if you can generate the document using the available characters. If you can generate the document, your function should return true; otherwise, it should return false.
- You're only able to generate the document if the frequency of unique characters in the characters string is greater than or equal to the frequency of unique characters in the document string. For example, if you're given characters = "abcabc" and document = "aabbccc" you cannot generate the document because you're missing one c.
- The document that you need to create may contain any characters, including special characters, capital letters, numbers, and spaces.
- Note: you can always generate the empty string ("").

**Sample Input**

```
characters = "Bste!hetsi ogEAxpelrt x "
document = "AlgoExpert is the Best!"
```

**Sample Output**

```
true
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> There are multiple ways to the solve this problem, but not all approaches have an optimal time complexity. Is there any way to solve this problem in better than O(m * (n + m)) or O(n * (n + m)) time, where n is the length of the characters string and m is the length of the document string? </strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> One of the simplest ways to solve this problem is to loop through the document string, one character at a time. At every character, you can count how many times it occurs in the document string and in the characters string. If it occurs more times in the document string than in the characters string, then you cannot generate the document. What is the time complexity of this approach? </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> The approach discussed in Hint #2 runs in O(m * (n + m)) time. Can you use some external space to optimize this time complexity? </strong></i>
</details>


<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> You can solve this problem in O(n + m) time. To do so, you need to use a hash table. Start by counting all of the characters in the characters string and storing these counts in a hash table. Then, loop through the document string, and check if each character is in the hash table and has a value greater than zero. If a character isn't in the hash table or doesn't have a value greater than zero, then you cannot generate the document. If a character is in the hash table and has a value greater than zero, then decrement its value in the hash table to indicate that you've "used" one of these available characters. If you make it through the entire document string without returning false, then you can generate the document.</strong></i>
</details>


<br>

## Method 1

```tex
【O(n+m)time∣O(c)space】
```

```tex
1.\ n\ is\ the\ number\ of\ characters\\
2.\ m\ is\ the\ length\ of\ the\ document\\
3.\ c\ is\ the\ number\ of\ unique\ characters\ in\ the\ characters\ string\
```

```java
package String;

import java.util.HashMap;

/**
 * @author zhengstars
 * @date 2023/05/15
 */
public class GenerateDocument {
    public static boolean generateDocument(String characters, String document) {
        // Create a HashMap to store the frequency of each character in the characters string
        HashMap<Character, Integer> charFrequency = new HashMap<>();
        // Traverse through the characters string and update the frequency of each character in the HashMap
        for (int i = 0; i < characters.length(); i++) {
            char c = characters.charAt(i);
            charFrequency.put(c, charFrequency.getOrDefault(c, 0) + 1);
        }

        // Traverse through the document string and check if each character is present in the characters string
        // Make sure that the frequency of each character in the document string is less than or equal to the frequency of the corresponding character in the characters string
        for (int i = 0; i < document.length(); i++) {
            char c = document.charAt(i);
            if (!charFrequency.containsKey(c) || charFrequency.get(c) == 0) {
                return false;
            }
            charFrequency.put(c, charFrequency.get(c) - 1);
        }

        return true;
    }

    public static void main(String[] args) {
        System.out.println(generateDocument("Bste!hetsi ogEAxpelrt x ","AlgoExpert is the Best!"));
    }
}
```

