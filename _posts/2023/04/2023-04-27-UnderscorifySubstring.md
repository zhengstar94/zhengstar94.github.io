---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Underscorify Substring"
date: "2023-04-27"
categories:
  - "Algorithm String"
---

# Underscorify Substring [Hard]

- Write a function that takes in two strings: a main string and a potential substring of the main string.
- The function should return a version of the main string with every instance of the substring in it wrapped between underscores.
- If two or more instances of the substring in the main string overlap each other or sit side by side, the underscores relevant to these substrings should only appear on the far left of the leftmost substring and on the far right of the rightmost substring. If the main string doesn't contain the other string at all, the function should return the main string intact.

**Sample Input **

```
string = "testthis is a testtest to see if testestest it works"
substring = "test"
```

**Sample Output **

```
"_test_this is a _testtest_ to see if _testestest_ it works"
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> The first thing you need to do to solve this question is to get the locations of all instances of the substring in the main string. Try traversing the main string one character at a time and calling whatever substring-matching function is built into the language you're working in. Store a 2D array of locations, where each subarray holds the starting and ending indices of a specific instance of the substring in the main string. </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The second thing you need to do is to "collapse"the 2D array mentioned in Hint #1. In essence, you need to merge the locations of substrings that overlap each other or sit next to each other. Traverse the 2D array mentioned in Hint #1 and build a new 2D array that holds these "collapsed" locations. </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Finally, you need to create a new string with underscores added in the correct positions. Construct this new string by traversing the main string and the 2D array mentioned in Hint #2 at the same time. You might have to keep track of when you are "in between"underscores in order to correctly traverse the 2D array. </strong></i>
</details>





<br>

## Method 1

```tex
【O(n+m)time∣O(n)space】
```

```java
package Sorting;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/06/03
 */
public class UnderscorifySubstring {
    public static String underscorifySubstring(String string, String substring) {
        // Get the locations of potential substrings in the main string
        List<int[]> locations = getSubstringLocations(string, substring);
        // Merge the locations to handle overlapping substrings
        List<int[]> mergedLocations = mergeLocations(locations);
        // Add underscores to the main string
        return underscorifyString(string, mergedLocations);
    }

    private static List<int[]> getSubstringLocations(String string, String substring) {
        List<int[]> locations = new ArrayList<>();
        int startIdx = 0;
        while (startIdx < string.length()) {
            // Find the next occurrence of the substring in the main string
            int nextIdx = string.indexOf(substring, startIdx);
            if (nextIdx != -1) {
                // Add the start and end indices of the substring to the locations list
                locations.add(new int[]{nextIdx, nextIdx + substring.length()});
                // Update the startIdx for the next search
                startIdx = nextIdx + 1;
            } else {
                break;
            }
        }
        return locations;
    }

    private static List<int[]> mergeLocations(List<int[]> locations) {
        if (locations.size() == 0) {
            return locations;
        }

        List<int[]> mergedLocations = new ArrayList<>();
        int[] currentLocation = locations.get(0);
        mergedLocations.add(currentLocation);

        for (int i = 1; i < locations.size(); i++) {
            int[] nextLocation = locations.get(i);
            if (nextLocation[0] <= currentLocation[1]) {
                // If the next location overlaps with the current location, update the end index of the current location
                currentLocation[1] = Math.max(currentLocation[1], nextLocation[1]);
            } else {
                // If there is no overlap, add the next location to the merged locations and update the current location
                mergedLocations.add(nextLocation);
                currentLocation = nextLocation;
            }
        }

        return mergedLocations;
    }

    private static String underscorifyString(String string, List<int[]> locations) {
        StringBuilder sb = new StringBuilder();
        int locationIdx = 0;
        int stringIdx = 0;
        boolean betweenUnderscores = false;

        while (stringIdx < string.length() && locationIdx < locations.size()) {
            int[] currentLocation = locations.get(locationIdx);
            if (stringIdx == currentLocation[0]) {
                // If the current position is the start index of a substring, decide whether to add an underscore based on whether we are currently between underscores
                if (betweenUnderscores) {
                    sb.append("_");
                    betweenUnderscores = false;
                } else {
                    sb.append("_");
                    betweenUnderscores = true;
                }
            } else if (stringIdx == currentLocation[1]) {
                // If the current position is the end index of a substring, add an underscore and move to the next location
                sb.append("_");
                betweenUnderscores = false;
                locationIdx++;
            }

            // Append the current character to the result string
            sb.append(string.charAt(stringIdx));
            stringIdx++;
        }

        // Handle the remaining cases
        if (locationIdx < locations.size()) {
            // If there are remaining locations, add an underscore
            sb.append("_");
        } else if (stringIdx < string.length()) {
            // If there are remaining characters, append them to the result string
            sb.append(string.substring(stringIdx));
        }

        return sb.toString();
    }

    public static void main(String[] args) {
        String string = "testthis is a testtest to see if testestest it works";
        String substring = "test";
        String result = underscorifySubstring(string, substring);
        System.out.println(result);
    }
}
```

