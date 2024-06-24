---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "190.Reverse Bits"
date: "2024-05-03"
categories:
  - "LeetCode BitManipulation"
---

# 190. Reverse Bits

- Reverse bits of a given 32 bits unsigned integer.

  **Note:**

  - Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
  - In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two's_complement). Therefore, in **Example 2** above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.

**Example 1**

```
Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
```

**Example 2**

```
Input: n = 11111111111111111111111111111101
Output:   3221225471 (10111111111111111111111111111111)
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.BitManipulation;

/**
 * @author zhengstars
 * @date 2024/06/10
 */
public class ReverseBits {

    /**
     * Reverse the bits of a given 32 bits unsigned integer.
     *
     * @param n the input 32 bits unsigned integer
     * @return the integer resulting from reversing the bits of {@code n}
     */
    public static int reverseBits(int n) {
        int result = 0;

        // Iterate over the 32 bits of the integer
        for (int i = 0; i < 32; i++) {
            // Left shift the result to make space for the next bit
            // This shifts all bits of result one place to the left
            result = result << 1;

            // Extract the lowest bit of n using bitwise AND with 1
            // If n's current lowest bit is 1, it returns 1; otherwise, it returns 0
            int lowestBit = n & 1;

            // Combine the shifted result with the lowest bit of n using bitwise OR
            // This is equivalent to adding the lowest bit of n to result's lowest place
            result = result | lowestBit;

            // Right shift n to discard the bit we just processed
            // This moves the next bit of n into the lowest place for the next iteration
            n = n >> 1;
        }

        return result;
    }

    /**
     * Reverse the bits of a given 32 bits unsigned integer using Java's built-in method.
     *
     * @param n the input 32 bits unsigned integer
     * @return the integer resulting from reversing the bits of {@code n}
     */
    public static int reverseBits2(int n) {
        // Use the Integer.reverse method which is a built-in utility to reverse bits
        return Integer.reverse(n);
    }

    public static void main(String[] args) {

        // Test case 1
        int n1 = 0b00000010100101000001111010011100;
        // expected output: 964176192 (0b00111001011110000010100101000000)
        System.out.println(reverseBits(n1));

        // Test case 2
        int n2 = 0b11111111111111111111111111111101;
        // expected output: 3221225471 (0b10111111111111111111111111111111)
        System.out.println(reverseBits(n2));
    }

}
```

