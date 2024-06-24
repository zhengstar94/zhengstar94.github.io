---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "191.Number of 1 Bits"
date: "2024-05-01"
categories:
  - "LeetCode BitManipulation"
---
# 191. Number of 1 Bits

- Write a function that takes the binary representation of a positive integer and returns the number of set bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

**Example 1**

```
Input: n = 11

Output: 3

Explanation:

The input binary string 1011 has a total of three set bits.
```

**Example 2**

```
Input: n = 128

Output: 1

Explanation:

The input binary string 10000000 has a total of one set bit.
```

**Example 3**

```
Input: n = 2147483645

Output: 30

Explanation:

The input binary string 1111111111111111111111111111101 has a total of thirty set bits.
```

## Method 1

```tex
【O(k) time | O(1) space】
```

```java
package Leetcode.BitManipulation;

/**
 * @author zhengstars
 * @date 2024/06/07
 */
public class NumberOfBits {
    // Function to calculate the number of 1 bits (Hamming Weight) of a positive integer.
    public static int hammingWeight(int n) {
        int count = 0;
        // Loop until n becomes 0
        while (n != 0) {
            // Eliminate the rightmost set bit (1) in the binary representation of n
            n &= (n - 1);
            // Increment the count as we have found a set bit
            count++;
        }
        // Return the total count of set bits
        return count;
    }

    // Method to test the function with given test cases
    public static void main(String[] args) {
        // Test case 1
        System.out.println(hammingWeight(11)); // Expected output: 3
        // Test case 2
        System.out.println(hammingWeight(128)); // Expected output: 1
        // Test case 3
        System.out.println(hammingWeight(2147483645)); // Expected output: 30
    }
}

```

