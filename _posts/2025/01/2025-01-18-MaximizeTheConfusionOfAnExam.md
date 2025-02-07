---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2024. Maximize the Confusion of an Exam"
date: "2025-01-18"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowMax"
---



- A teacher is writing a test with `n` true/false questions, with `'T'` denoting true and `'F'` denoting false. He wants to confuse the students by **maximizing** the number of **consecutive (adj. 连续的；连贯的)** questions with the **same** answer (multiple trues or multiple falses in a row).
- You are given a string `answerKey`, where `answerKey[i]` is the original answer to the `ith` question. In addition, you are given an integer `k`, the maximum number of times you may perform the following operation:
  - Change the answer key for any question to `'T'` or `'F'` (i.e., set `answerKey[i]` to `'T'` or `'F'`).
- Return *the **maximum** number of consecutive (adj. 连续的；连贯的)* `'T'`s or `'F'`s *in the answer key after performing the operation at most* `k` *times*.

**Example 1**

```
Input: answerKey = "TTFF", k = 2
Output: 4
Explanation: We can replace both the 'F's with 'T's to make answerKey = "TTTT".
There are four consecutive 'T's.
```

**Example 2**

```
Input: answerKey = "TFFT", k = 1
Output: 3
Explanation: We can replace the first 'T' with an 'F' to make answerKey = "FFFT".
Alternatively, we can replace the second 'T' with an 'F' to make answerKey = "TFFF".
In both cases, there are three consecutive 'F's.
```

**Example 3**

```
Input: answerKey = "TTFTTFTT", k = 1
Output: 5
Explanation: We can replace the first 'F' to make answerKey = "TTTTTFTT"
Alternatively, we can replace the second 'F' to make answerKey = "TTFTTTTT". 
In both cases, there are five consecutive 'T's.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/18
 */
public class MaximizeTheConfusionOfAnExam {
    public static int maxConsecutiveAnswers(String answerKey, int k) {
        // Calculate max length by considering both cases: converting to 'T' and converting to 'F'
        // Return the maximum of these two scenarios
        return Math.max(maxLength(answerKey, k, 'T'), maxLength(answerKey, k, 'F'));
    }

    private static int maxLength(String answerKey, int k, char target) {
        int maxLen = 0;        // Track the maximum length of consecutive same characters
        int left = 0;          // Left pointer of sliding window
        int count = 0;         // Count of characters that need to be changed in current window

        for (int right = 0; right < answerKey.length(); right++) {
            // Step 1: Process the current character at right pointer
            // If current character is different from target, increment the count
            // This count represents how many characters we need to change in our window
            if (answerKey.charAt(right) != target) {
                count++;
            }

            // Step 2: Maintain window validity
            // If count exceeds k, we need to shrink the window from left
            // Keep shrinking until we have a valid window (count <= k)
            while (count > k) {
                // If character at left pointer is different from target
                // Decrease count as this character will no longer be in our window
                if (answerKey.charAt(left) != target) {
                    count--;
                }
                // Move left pointer to shrink window
                left++;
            }

            // Step 3: Update result
            // Current window size is (right - left + 1)
            // Update maxLen if current window is larger
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }

    public static void main(String[] args) {
        // Test cases to verify the solution
        System.out.println(maxConsecutiveAnswers("TTFF", 2));     // Output: 4
        System.out.println(maxConsecutiveAnswers("TFFT", 1));     // Output: 3
        System.out.println(maxConsecutiveAnswers("TTFTTFTT", 1)); // Output: 5
    }
}

```





