---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Run-Length Encoding"
date: "2023-04-16"
categories:
  - "Algorithm String"
---


# Run-Length Encoding [Easy]

- Write a function that takes in a non-empty string and returns its run-length encoding.
- From Wikipedia, "run-length encoding is a form of lossless data compression in which runs of data are stored as a single data value and count, rather than as the original run." For this problem, a run of data is any sequence of consecutive, identical characters. So the run "AAA" would be run-length-encoded as "3A".
- To make things more complicated, however, the input string can contain all sorts of special characters, including numbers. And since encoded data must be decodable, this means that we can't naively run-length-encode long runs. For example, the run "AAAAAAAAAAAA" (12 As), can't naively be encoded as "12A", since this string can be decoded as either "AAAAAAAAAAAA" or "1AA". Thus, long runs (runs of 10 or more characters) should be encoded in a split fashion; the aforementioned run should be encoded as "9A3A".

**Sample Input**

```
string = "AAAAAAAAAAAAABBCCCCDD"
```

**Sample Output**

```
"9A4A2B4C2D"
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Traverse the input string and count the length of each run. As you traverse the string, what should you do when you reach a run of length 9 or the end of a run? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> When you reach a run of length 9 or the end of a run, store the computed count for the run as well as its character (you'll likely need a list for these computed counts and characters), and reset the count to 1 before continuing to traverse the string. </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Make sure that your solution correctly handles the last run in the string. </strong></i>
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
public class RunLengthEncoding {
    public static String runLengthEncoding(String str) {
        // Used to store the encoded string
        StringBuilder encodedStr = new StringBuilder();
        // Number of consecutive occurrences of the current character
        int count = 1;
        // Previous character
        char prevChar = str.charAt(0);

        for (int i = 1; i < str.length(); i++) {
            // Current character
            char currChar = str.charAt(i);

            // If current character is different from previous character or count reaches 9
            if (currChar != prevChar || count == 9) {
                // Append encoding of previous character to encodedStr
                encodedStr.append(count).append(prevChar);
                // Reset count
                count = 1;
            } else {
                // Increment count by 1
                count++;
            }
            // Update prevChar to current character
            prevChar = currChar;
        }

        // Append encoding of last character to encodedStr
        encodedStr.append(count).append(prevChar);
        // Return the encoded string
        return encodedStr.toString();
    }

    public static void main(String[] args) {
        System.out.println(runLengthEncoding("AAAAAAAAAAAAABBCCCCDD"));
    }
}
```

