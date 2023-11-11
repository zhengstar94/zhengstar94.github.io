# Stable Internships [Medium]

- There are N interns and M companies. Each intern has a ranked list of their preferred companies, and each company has a ranked list of their preferred interns.
- Write an algorithm to produce a stable matching between interns and companies. A stable matching satisfies the following conditions:
    1. Every intern is matched to one company.
    2. No intern-company pair that is matched would both prefer to be matched to someone else.

- The input is two 2D arrays:
    - `interns[i] = [c1, c2, c3]` represents intern i's ranked list of companies in order of preference.
    - `companies[j] = [i1, i2, i3]` represents company j's ranked list of interns in order of preference.

The output is a 2D array containing stable intern-company pairs, with each pair in the form [internIndex, companyIndex].

**Sample Input:**

```java
interns = [[0, 1, 2], [1, 0, 2], [2, 1, 0]]
companies = [[2, 0, 1], [0, 2, 1], [1, 2, 0]]
```

**Sample Output:**

```java
[[0, 2], [1, 0], [2, 1]]
```

This translates to:

- Intern 0 matched with Company 2
- Intern 1 matched with Company 0
- Intern 2 matched with Company 1

<br>



## Method 1

```tex
【O(n^2)time∣O(n^2)space】
```

```java
package Famous;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/11/11
 */
public class StableInternships {

    /**
     * Stable Matching Algorithm
     * @param interns
     * @param teams
     * @return
     */
    public static int[][] stableInternships(int[][] interns, int[][] teams) {

        // Time complexity: O(n^2) Space complexity: O(n^2), where n is the number of interns and companies

        // Map to store the mapping between matched companies and interns
        Map<Integer, Integer> teamsAndInterns = new HashMap<>();

        // Stack to store unmatched interns
        Stack<Integer> freeInterns = new Stack<>();

        // Number of interns
        int internsLength = interns.length;

        // Add intern numbers to the stack. For example, if there are 3 interns, the stack elements will be 2, 1, 0.
        for (int i = 0; i < internsLength; i++) {
            freeInterns.push(i);
        }

        // List to store the ranking information of companies for each intern
        List<Map<Integer, Integer>> teamMaps = new ArrayList<>();

        // Iterate through each company
        for (int[] team : teams) {

            // Map to store the current company's ranking information for interns
            Map<Integer, Integer> rank = new HashMap<>();

            // Iterate through the array representing the company's ranking of interns
            for (int i = 0; i < team.length; i++) {
                // Put the intern's number as the key and the ranking as the value into the map
                rank.put(team[i], i);
            }

            // Add the current company's ranking information map to the list
            teamMaps.add(rank);
        }

        // Array to store the current index of the ranking chosen by each intern
        int[] teamsChosenByCurrentIntern = new int[internsLength];

        // Initialize to 0, indicating that each intern starts by choosing their first preferred company

        // Continue matching while there are still unmatched interns
        while (!freeInterns.isEmpty()) {

            // Pop an unmatched intern
            int internNum = freeInterns.pop();

            // Get the current intern's company preferences
            int[] teamPreferences = interns[internNum];

            // Get the company currently chosen by the intern
            int teamPreference = teamPreferences[teamsChosenByCurrentIntern[internNum]];

            // Update the intern's choice order
            teamsChosenByCurrentIntern[internNum]++;

            // If the current company has not been chosen by other interns yet
            if (!teamsAndInterns.containsKey(teamPreference)) {
                // Match the current intern with the company
                teamsAndInterns.put(teamPreference, internNum);
                continue;
            }

            // If the current company has been chosen by other interns
            int previousIntern = teamsAndInterns.get(teamPreference);
            int previousInternRank = teamMaps.get(teamPreference).get(previousIntern);
            int currentInternRank = teamMaps.get(teamPreference).get(internNum);

            // Compare the ranking of the current intern and the previously chosen intern in the company
            if (currentInternRank < previousInternRank) {
                // If the current intern has a higher ranking, reinsert the previous intern into the stack
                freeInterns.push(previousIntern);
                // Update the matching relationship
                teamsAndInterns.put(teamPreference, internNum);
            } else {
                // If the previously chosen intern has a higher ranking, reinsert the current intern into the stack
                freeInterns.push(internNum);
            }
        }

        // Store the matching results in a two-dimensional array and return
        int[][] matches = new int[internsLength][2];
        int index = 0;

        for (Map.Entry<Integer, Integer> teamAndIntern : teamsAndInterns.entrySet()) {
            matches[index] = new int[]{teamAndIntern.getValue(), teamAndIntern.getKey()};
            index++;
        }

        return matches;
    }

    public static void main(String[] args) {
        System.out.println(Arrays.deepToString(
                stableInternships(
                        new int[][]{
                                {0, 1, 2},
                                {1, 0, 2},
                                {1, 2, 0}},
                        new int[][]{
                                {2, 1, 0},
                                {1, 2, 0},
                                {0, 2, 1}})
        ));
    }
}

```