---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Valid IP Addresses"
date: "2023-04-22"
categories:
  - "Algorithm String"
---

# Valid IP Addresses [Medium]

- You're given a string of length 12 or smaller, containing only digits. Write a function that returns all the possible IP addresses that can be created by inserting three .s in the string.
- An IP address is a sequence of four positive integers that are separated by .s, where each individual integer is within the range 0 - 255, inclusive.
- An IP address isn't valid if any of the individual integers contains leading 0s. For example, "192.168.0.1" is a valid IP address, but "192.168.00.1" and "192.168.0.01" aren't, because they contain "00" and 01, respectively. Another example of a valid IP address is "99.1.1.10"; conversely, "991.1.1.0" isn't valid, because "991" is greater than 255.
- Your function should return the IP addresses in string format and in no particular order. If no valid IP addresses can be created from the string, your function should return an empty list.
- Note: check out our Systems Design Fundamentals on SystemsExpert to learn more about IP addresses!

**Sample Input**

```
string = "1921680"
```

**Sample Output**

```
[
    "1.9.216.80",
    "1.92.16.80",
    "1.92.168.0",
    "19.2.16.80",
    "19.2.168.0",
    "19.21.6.80",
    "19.21.68.0",
    "19.216.8.0",
    "192.1.6.80",
    "192.1.68.0",
    "192.16.8.0"
]
// The IP addresses could be ordered differently.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> How can you split this problem into subproblems to make it easier? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Each IP address is comprised of four parts; try finding one valid part at a time and then combining sets of four valid parts to create one valid IP address. </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Go through all possible combinations of valid IP-address parts. You'll do this by generating a valid first part, then generating all valid second parts given the first part, then finally all valid third and fourth parts given first and second parts. If you find a set of four valid parts, then simply combine them together and add that IP address to some final array. You can start by creating all the possible first parts of an IP address; these will be substrings of the main string that start at the first character and that have lengths 1, 2 and 3. Then you can repeat this process for the second part, where the substrings in this part will start where the first part ended. The same thing applies for the third and fourth parts. After going through all possible parts and storing valid IP addresses, you'll have found all of the IP addresses that can be formed from the input string. </strong></i>
</details>

<br>

## Method 1

```tex
【O(1)time∣O(1)space】
```

```java
package String;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/05/25
 */
public class ValidIPAddresses {
    public static List<String> validIPAddresses(String string) {
        List<String> result = new ArrayList<>();
        // If the string is null, or its length is less than 4 or greater than 12, return an empty list
        if (string == null || string.length() < 4 || string.length() > 12) {
            return result;
        }

        // The first segment of the IP address can have at most 3 digits
        for (int i = 1; i < 4 && i < string.length(); i++) {
            String first = string.substring(0, i);
            // If the first segment is not valid, skip it
            if (!isValid(first)) {
                continue;
            }

            // The second segment of the IP address can have at most 3 digits
            for (int j = i + 1; j < i + 4 && j < string.length(); j++) {
                String second = string.substring(i, j);
                // If the second segment is not valid, skip it
                if (!isValid(second)) {
                    continue;
                }

                // The third segment of the IP address can have at most 3 digits
                for (int k = j + 1; k < j + 4 && k < string.length(); k++) {
                    String third = string.substring(j, k);
                    // The fourth segment starts after the third segment
                    String fourth = string.substring(k);
                    // If both the third and fourth segments are valid, add the IP address to the result list
                    if (isValid(third) && isValid(fourth)) {
                        result.add(first + "." + second + "." + third + "." + fourth);
                    }
                }
            }
        }
        return result;
    }

    /**
     * Checks if a string is a valid segment of an IP address
     *
     * @param s The input string
     * @return True if the string is a valid IP address segment, False otherwise
     */
    private static boolean isValid(String s) {
        // If the string is null, has a length of 0, or its length is greater than 3, it is not valid
        if (s == null || s.length() == 0 || s.length() > 3) {
            return false;
        }
        // If the first character is '0' and the length is greater than 1, it is not valid
        if (s.charAt(0) == '0' && s.length() > 1) {
            return false;
        }
        // Convert the string to an integer
        int num = Integer.parseInt(s);
        // If the integer is between 0 and 255 (inclusive), it is a valid segment
        return num >= 0 && num <= 255;
    }

    public static void main(String[] args) {
        String str = "1921680";
        List<String> result = validIPAddresses(str);
        System.out.println(result);
    }
}
```

