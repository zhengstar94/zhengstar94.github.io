---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "659.Encode and Decode Strings"
date: "2024-02-03"
categories:
  - "LeetCode Array"
---

# LeetCode 659. Encode and Decode Strings [Easy]

- Design an algorithm to encode a list of strings to a string.The encoded string is then sent over the network and is decoded back to the original list of strings.
- Please implement encode and decode.

**Example 1**

```
Input:["lint","code","love","you"] 
Output:["lint","code","love","you"]
Explanation:
One possible encode method is: "lint:;code:;love:;you"
```

**Example 2**

```
Input:["we", "say", ":", "yes"]
Output:["we", "say", ":", "yes"]
Explanation:
One possible encode method is: "we:;say:;:::;yes"
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
/**
 * @author zhengstars
 * @date 2024/02/28
 */
public class EncodeAndDecodeStrings {
    public static String encode(List<String> strs) {
  // Initialize a StringBuilder to construct the encoded string.
    StringBuilder sb = new StringBuilder();
    
    // Iterate over each string in input list.
    for (String str : strs) {
      // Append the string's length and a special character '#' as delimiter to the StringBuilder.
      sb.append(str.length()).append('#');
      // Append the current string to the StringBuilder.
      sb.append(str);
    }
    
    // Return the final encoded string.
    return sb.toString();
  }

  // Decodes a single string to a list of strings.
  public static List<String> decode(String s) {
  // Initialize a list to hold the decoded strings.
    List<String> strs = new ArrayList<>();
    
    // Start index for scanning the string.
    int i = 0;
    
    // Scan the encoded string until end.
    while (i < s.length()) {
      // Find the position of the next delimiter #.
      int delimiterIndex = s.indexOf('#', i);
      
      // Parse the length of the current segment.
      int len = Integer.parseInt(s.substring(i, delimiterIndex));
      
      // Extract the string using the length.
      String str = s.substring(delimiterIndex + 1, delimiterIndex + 1 + len);
      
      // Add the substring to the list of decoded strings.
      strs.add(str);
      
      // Update the start index to the character after the current segment.
      i = delimiterIndex + 1 + len;
    }
    
    // Returns the list of decoded strings.
    return strs;
  }

  // Test cases for the codec functions.
  public static void main(String[] args) {
    // Test with the input ["lint", "code", "love", "you"].
    List<String> input = Arrays.asList("lint", "code", "love", "you");
    String encodedString = encode(input);
    System.out.println("Encoded string: " + encodedString);
    List<String> decodedList = decode(encodedString);
    System.out.println("Decoded list: " + decodedList);

    // Test with the input ["we", "say", ":", "yes"].
    List<String> input2 = Arrays.asList("we", "say", ":", "yes");
    String encodedString2 = encode(input2);
    System.out.println("Encoded string 2: " + encodedString2);
    List<String> decodedList2 = decode(encodedString2);
    System.out.println("Decoded list 2: " + decodedList2);
  }
}
```

