---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2269. Find the K-Beauty of a Number"
date: "2025-01-07"
tags: Easy SlideWindow
categories:
  - "LeetCode SlideWindow"
---


- The **k-beauty** of an integer `num` is defined as the number of **substrings** of `num` when it is read as a string that meet the following conditions:
  - It has a length of `k`.
  - It is a divisor of `num`.
- Given integers `num` and `k`, return *the k-beauty of* `num`.
- Note:
  - **Leading zeros** are allowed.
  - `0` is not a divisor of any value.
- A **substring** is a contiguous sequence of characters in a string.

**Example 1**

```
Input: num = 240, k = 2
Output: 2
Explanation: The following are the substrings of num of length k:
- "24" from "240": 24 is a divisor of 240.
- "40" from "240": 40 is a divisor of 240.
Therefore, the k-beauty is 2.
```

**Example 2**

```
Input: num = 430043, k = 2
Output: 2
Explanation: The following are the substrings of num of length k:
- "43" from "430043": 43 is a divisor of 430043.
- "30" from "430043": 30 is not a divisor of 430043.
- "00" from "430043": 0 is not a divisor of 430043.
- "04" from "430043": 4 is not a divisor of 430043.
- "43" from "430043": 43 is a divisor of 430043.
Therefore, the k-beauty is 2.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/07
 */
public class FindTheKBeautyOfANumber {
    public static int divisorSubstrings(int num, int k) {
        String numStr = String.valueOf(num);
        int n = numStr.length();
        int count = 0;

        for (int i = 0; i <= n - k; i++) {
            // Extract substring of length k starting from index i
            String substring = numStr.substring(i, i + k);
            // Convert substring to integer for division check
            int subNum = Integer.parseInt(substring);

            // Check if subNum is not zero and is a divisor of num
            if (subNum != 0 && num % subNum == 0){
                count++;
            }
        }
        return count;
    }

    public static void main(String[] args) {
        // Test case 1
        int num1 = 240;
        int k1 = 2;
        System.out.println("Test case 1:");
        System.out.println("Input: num = " + num1 + ", k = " + k1);
        System.out.println("Output: " + divisorSubstrings(num1, k1));
        System.out.println("Expected result: 2");

        // Test case 2
        int num2 = 430043;
        int k2 = 2;
        System.out.println("\nTest case 2:");
        System.out.println("Input: num = " + num2 + ", k = " + k2);
        System.out.println("Output: " + divisorSubstrings(num2, k2));
        System.out.println("Expected result: 2");
    }
}
```





