---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Caesar Cipher Encryptor"
date: "2023-04-14"
categories:
  - "Algorithm String"
---

# Caesar Cipher Encryptor [Easy]

- Given a non-empty string of lowercase letters and a non-negative integer representing a key, write a function that returns a new string obtained by shifting every letter in the input string by k positions in the alphabet, where k is the key.
- Note that letters should "wrap" around the alphabet; in other words, the letter z shifted by one returns the letter a.

**Sample Input**

```
string = "xyz"
key = 2
```

**Sample Output**

```
"zab"
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Most languages have built-in functions that give you the Unicode value of a character as well as the character corresponding to a Unicode value. Consider using such functions to determine which letters the input string's letters should be mapped to. </strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Try creating your own mapping of letters to codes. In other words, try associating each letter in the alphabet with a specific number - its position in the alphabet, for instance - and using that to determine which letters the input string's letters should be mapped to. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> How do you handle cases where a letter gets shifted to a position that requires wrapping around the alphabet? What about cases where the key is very large and causes multiple wrappings around the alphabet? The modulo operator should be your friend here.</strong></i>
</details>



<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package String;

/**
 * @author zhengstars
 * @date 2023/05/14
 */
public class CaesarCipherEncryptor2 {
    public static String caesarCypherEncryptor(String str, int key) {
        // Take the key mod 26, because when key = 26, it means no encryption.
        key = key % 26;
        // Create a character array to store the encrypted string.
        char[] characters = new char[str.length()];

        // Traverse each character in the original string and store the encrypted character in the character array.
        for(int i = 0; i < characters.length; i++){
            characters[i] = getNewLetter(str.charAt(i), key);
        }

        // Convert the character array to a string and return.
        return new String(characters);
    }

    public static char getNewLetter(char letter, int key){
        // Convert the character to ASCII code, add the key to get the ASCII code of the new character.
        int newLetterNum = (int) letter + key;
        // If the ASCII code of the new character is beyond the range of lowercase letters (97 ~ 122), it needs to wrap around to a ~ z.
        if(newLetterNum > 122){
            newLetterNum = newLetterNum % 122 + 96;
        }

        // Convert the ASCII code of the new character back to a character and return.
        return (char) newLetterNum;
    }

    public static void main(String[] args) {
        System.out.println(caesarCypherEncryptor("xyz",2));
    }
}
```

