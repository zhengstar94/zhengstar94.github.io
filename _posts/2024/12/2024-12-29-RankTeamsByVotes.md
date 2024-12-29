---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1366. Rank Teams by Votes"
date: "2024-12-29"
tags: Medium
categories:
  - "LeetCode Array"
---

- In a special ranking system, each voter gives a rank from highest to lowest to all teams participating in the competition.
- The ordering of teams is decided by who received the most position-one votes. If two or more teams tie in the first position, we consider the second position to resolve the conflict, if they tie again, we continue this process until the ties are resolved. If two or more teams are still tied after considering all positions, we rank them alphabetically based on their team letter.
- You are given an array of strings `votes` which is the votes of all voters in the ranking systems. Sort all teams according to the ranking system described above.
- Return *a string of all teams **sorted** by the ranking system*.

**Example 1**

```
Input: votes = ["ABC","ACB","ABC","ACB","ACB"]
Output: "ACB"
Explanation: 
Team A was ranked first place by 5 voters. No other team was voted as first place, so team A is the first team.
Team B was ranked second by 2 voters and ranked third by 3 voters.
Team C was ranked second by 3 voters and ranked third by 2 voters.
As most of the voters ranked C second, team C is the second team, and team B is the third.
```

**Example 2**

```
Input: votes = ["WXYZ","XYZW"]
Output: "XWYZ"
Explanation:
X is the winner due to the tie-breaking rule. X has the same votes as W for the first position, but X has one vote in the second position, while W does not have any votes in the second position. 
```

**Example 3**

```
Input: votes = ["ZMNAGUEDSJYLBOPHRQICWFXTVK"]
Output: "ZMNAGUEDSJYLBOPHRQICWFXTVK"
Explanation: Only one voter, so their votes are used for the ranking.
```

## Method 1

```tex
【O(N * K + M * log M) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/29
 */
public class RankTeamsByVotes {
    public static String rankTeams(String[] votes) {
        // Handle edge cases: empty input or null
        if(votes == null || votes.length == 0){
            return "";
        }

        // If there's only one vote, return it directly
        if (votes.length == 1){
            return votes[0];
        }

        // Create a 2D array to store vote counts
        // First dimension [26]: represents teams (A-Z)
        // Second dimension [26]: represents positions (0 to 25)
        // count[team][position] stores how many votes a team got for that position
        int[][] count = new int[26][26];

        // Array to track which teams participated in voting
        // teams[i] will be true if team (A+i) received any votes
        boolean[] teams = new boolean[26];
        int teamCount = 0;

        /* First Important Section: Vote Counting Loop
         * Outer loop: Iterates through each vote (ballot)
         * Inner loop: Processes each position in the current vote
         *
         * For example, if vote is "ABC":
         * - First iteration (i=0): Increment count for A in position 0
         * - Second iteration (i=1): Increment count for B in position 1
         * - Third iteration (i=2): Increment count for C in position 2
         *
         * Also marks teams as participating when they first appear
         */
        for (String vote : votes) {
            for (int i = 0; i < vote.length(); i++) {
                int team = vote.charAt(i) - 'A';  // Convert character to 0-25 index
                count[team][i]++;                 // Increment vote count for team at position i
                if (!teams[team]) {               // If this is first time seeing this team
                    teams[team] = true;           // Mark team as participating
                    teamCount++;                  // Increment total number of teams
                }
            }
        }

        // Create array to store participating teams as characters
        Character[] result = new Character[teamCount];
        int index = 0;
        for (int i = 0; i < 26; i++) {
            if (teams[i]) {
                result[index++] = (char)('A' + i);
            }
        }

        /* Second Important Section: Custom Sorting Logic
         * This comparator implements the ranking rules:
         * 1. Primary Rule: Teams are ranked by votes at each position, starting from position 0
         * 2. Secondary Rule: If teams tie in all positions, sort alphabetically
         *
         * For example, comparing teams A and B:
         * - First checks votes in position 0
         * - If tied, checks position 1
         * - If tied, checks position 2
         * - If tied in all positions, compares team names (A vs B)
         *
         * The subtraction (count[teamB][i] - count[teamA][i]) ensures:
         * - Positive result: teamB ranks higher (has more votes)
         * - Negative result: teamA ranks higher
         * - Zero: tied at this position, continue checking
         */
        Arrays.sort(result, (a, b) -> {
            int teamA = a - 'A';
            int teamB = b - 'A';

            // Compare votes at each position
            for (int i = 0; i < 26; i++) {
                if (count[teamA][i] != count[teamB][i]) {
                    return count[teamB][i] - count[teamA][i];  // Higher votes first
                }
            }
            // If all positions are tied, sort alphabetically
            return a - b;
        });

        // Convert result array to string
        StringBuilder sb = new StringBuilder();
        for (char c : result) {
            sb.append(c);
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        // Test case 1: Multiple votes with clear preference
        String[] votes1 = {"ABC","ACB","ABC","ACB","ACB"};
        System.out.println("Test case 1:");
        System.out.println("Input: " + Arrays.toString(votes1));
        System.out.println("Output: " + rankTeams(votes1));
        System.out.println("Expected: ACB");

        // Test case 2: Minimal votes with different preferences
        String[] votes2 = {"WXYZ","XYZW"};
        System.out.println("\nTest case 2:");
        System.out.println("Input: " + Arrays.toString(votes2));
        System.out.println("Output: " + rankTeams(votes2));
        System.out.println("Expected: XWYZ");

        // Test case 3: Single vote with all letters
        String[] votes3 = {"ZMNAGUEDSJYLBOPHRQICWFXTVK"};
        System.out.println("\nTest case 3:");
        System.out.println("Input: " + Arrays.toString(votes3));
        System.out.println("Output: " + rankTeams(votes3));
        System.out.println("Expected: ZMNAGUEDSJYLBOPHRQICWFXTVK");
    }
}
```





