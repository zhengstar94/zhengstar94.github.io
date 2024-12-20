---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "Master Fixed-Length Sliding Window: A Universal Approach"
date: "2024-12-20"
categories: 
  - "Data Structure"
---

------

## **Core Concept**

We aim to calculate the maximum number of vowels in any substring with a length of exactly *k*. While brute-forcing all substrings results in a time complexity of O(*nk*), this approach is too slow. Can we achieve O(1) substring property calculations? Yes! For instance:

In the string "abci", if we already know the vowel count in substring "abc", then to compute it for "bci":

1. Check if the leaving character ('a') is a vowel.
2. Check if the entering character ('i') is a vowel.

This works because the middle characters ('b' and 'c') remain unchanged in both substrings.

------

## **Example Walkthrough**

**Input:** *s* = "abciiidef", *k* = 3

**Step-by-step:**

1. Traverse *s* from left to right.
2. Count vowels in the first *k* − 1 = 2 characters. Initially, there is 1 vowel.
3. Start processing the sliding window:
  - *s*[2] = 'c' enters window, forming "abc" (1 vowel). Update max count. Then *s*[0] = 'a' exits window, reducing the count to 0.
  - *s*[3] = 'i' enters window, forming "bci" (1 vowel). Update max count. Then *s*[1] = 'b' exits, keeping the count at 1.
  - *s*[4] = 'i' enters window, forming "cii" (2 vowels). Update max count. Then *s*[2] = 'c' exits, keeping the count at 2.
  - *s*[5] = 'i' enters window, forming "iii" (3 vowels). Update max count. Then *s*[3] = 'i' exits, reducing the count to 2.
  - *s*[6] = 'd' enters window, forming "iid" (2 vowels). Update max count. Then *s*[4] = 'i' exits, reducing the count to 1.
  - *s*[7] = 'e' enters window, forming "ide" (2 vowels). Update max count. Then *s*[5] = 'i' exits, reducing the count to 1.
  - *s*[8] = 'f' enters window, forming "def" (1 vowel). Update max count. Traversal complete.

------

## **Fixed-Length Sliding Window Pattern**

This pattern follows three simple steps: **Enter-Update-Exit**

1. **Enter**: Element at index *i* enters the window; update statistics. Repeat if *i* < *k* − 1.
2. **Update**: Update the answer (usually max/min value).
3. **Exit**: Element at index *i* − *k* + 1 exits the window; update statistics.

This pattern is universally applicable to all fixed-length sliding window problems.

------

## **Implementation**

```java
class Solution {
    public int maxVowels(String S, int k) {
        char[] s = S.toCharArray();
        int ans = 0;
        int vowel = 0;
        for (int i = 0; i < s.length; i++) {
            // 1. Enter window
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' ||
                s[i] == 'o' || s[i] == 'u') {
                vowel++;
            }
            if (i < k - 1) { // Window size less than k
                continue;
            }
            // 2. Update answer
            ans = Math.max(ans, vowel);
            // 3. Exit window
            char out = s[i - k + 1];
            if (out == 'a' || out == 'e' || out == 'i' ||
                out == 'o' || out == 'u') {
                vowel--;
            }
        }
        return ans;
    }
}
```

------

## **Complexity Analysis**

- **Time Complexity:** O(*n*), where *n* is the length of *s*.
- **Space Complexity:** O(1), using only a few extra variables.

------

## **Key Benefits**

1. Universal applicability to fixed-length sliding window problems.
2. Simple and easy-to-remember three-step process: **Enter-Update-Exit**.
3. Efficient O(*n*) time complexity.
4. Minimal space usage (O(1)).

------

## **Best Practices**

1. Initialize your window with the first *k* − 1 elements.
2. Update the answer after forming the complete window.
3. Update statistics for both entering and exiting elements.
4. Handle boundary conditions carefully, especially during window formation.

------

## **Applications**

This pattern can be adapted for various fixed-length sliding window problems, such as:

- Finding the maximum/minimum sum of *k* consecutive elements.
- Finding the maximum/minimum average of *k* consecutive elements.
- Counting occurrences of specific patterns in *k*-length windows.
- Calculating statistics over *k*-length sliding windows.

Mastering this pattern empowers you to solve a wide range of problems efficiently and effectively!


