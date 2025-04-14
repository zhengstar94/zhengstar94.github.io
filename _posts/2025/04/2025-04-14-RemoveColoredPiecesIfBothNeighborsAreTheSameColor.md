---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2038. Remove Colored Pieces if Both Neighbors are the Same Color"
date: "2025-04-14"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---

- There are `n` pieces arranged in a line, and each piece is colored either by `'A'` or by `'B'`. You are given a string `colors` of length `n` where `colors[i]` is the color of the `ith` piece.
- Alice and Bob are playing a game where they take **alternating turns** removing pieces from the line. In this game, Alice moves **first**.
  - Alice is only allowed to remove a piece colored `'A'` if **both its neighbors** are also colored `'A'`. She is **not allowed** to remove pieces that are colored `'B'`.
  - Bob is only allowed to remove a piece colored `'B'` if **both its neighbors** are also colored `'B'`. He is **not allowed** to remove pieces that are colored `'A'`.
  - Alice and Bob **cannot** remove pieces from the edge of the line.
  - If a player cannot make a move on their turn, that player **loses** and the other player **wins**.
- Assuming Alice and Bob play optimally, return `true` *if Alice wins, or return* `false` *if Bob wins*.

**Example 1**

```
Input: colors = "AAABABB"
Output: true
Explanation:
AAABABB -> AABABB
Alice moves first.
She removes the second 'A' from the left since that is the only 'A' whose neighbors are both 'A'.

Now it's Bob's turn.
Bob cannot make a move on his turn since there are no 'B's whose neighbors are both 'B'.
Thus, Alice wins, so return true.
```

**Example 2**

```
Input: colors = "AA"
Output: false
Explanation:
Alice has her turn first.
There are only two 'A's and both are on the edge of the line, so she cannot move on her turn.
Thus, Bob wins, so return false.
```

**Example 2**

```
Input: colors = "ABBBBBBBAAA"
Output: false
Explanation:
ABBBBBBBAAA -> ABBBBBBBAA
Alice moves first.
Her only option is to remove the second to last 'A' from the right.

ABBBBBBBAA -> ABBBBBBAA
Next is Bob's turn.
He has many options for which 'B' piece to remove. He can pick any.

On Alice's second turn, she has no more pieces that she can remove.
Thus, Bob wins, so return false.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/14
 */
public class RemoveColoredPiecesIfBothNeighborsAreTheSameColor {

    public static boolean winnerOfGame(String colors) {
        // Handle edge cases: if string is null or length < 3, no valid moves possible
        if (colors == null || colors.length() < 3){
            return false;
        }

        // Counter for number of possible moves for each player
        // alice: represents how many 'A' pieces Alice can remove
        // bob: represents how many 'B' pieces Bob can remove
        int alice = 0;
        int bob = 0;

        // Convert string to char array for easier processing
        char[] chars = colors.toCharArray();
        int i = 0;  // Current position in the array
        int n = colors.length();  // Length of the input string

        // Use grouped loop to process consecutive same-colored pieces
        while (i < n){
            int start = i;  // Start position of current group
            char c = chars[i];  // Current color being processed

            // Count consecutive pieces of the same color
            while (i < n && chars[i] == c){
                i++;
            }

            // Calculate length of current group
            int len = i - start;

            // If group length >= 3, calculate number of possible moves
            // For each group of length n, number of possible moves is (n-2)
            // because end pieces cannot be removed
            if (len >= 3){
                if (c == 'A'){
                    alice += len - 2;  // Add possible moves for Alice
                } else{
                    bob += len - 2;   // Add possible moves for Bob
                }
            }
        }

        // Alice wins if she has more possible moves than Bob
        // This works because:
        // 1. Alice goes first
        // 2. Each player will always make a move if possible
        // 3. The player with more moves will be the last to make a move
        return alice > bob;
    }

    public static void main(String[] args) {
        // Test case 1: Alice should win
        String colors1 = "AAABABB";
        System.out.println("Test case 1: " + winnerOfGame(colors1)); // Expected output: true

        // Test case 2: Bob should win (Alice has no valid moves)
        String colors2 = "AA";
        System.out.println("Test case 2: " + winnerOfGame(colors2)); // Expected output: false

        // Test case 3: Bob should win (Bob has more possible moves)
        String colors3 = "ABBBBBBBAAA";
        System.out.println("Test case 3: " + winnerOfGame(colors3)); // Expected output: false
    }
}

```





