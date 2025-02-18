---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1868. Product of Two Run-Length Encoded Arrays"
date: "2024-11-17"
categories:
  - "LeetCode TwoPointers"
---

- **Run-length encoding** is a compression algorithm that allows for an integer array `nums` with many segments of **consecutive repeated** numbers to be represented by a (generally smaller) 2D array `encoded`. Each `encoded[i] = [vali, freqi]` describes the `ith` segment of repeated numbers in `nums` where `vali` is the value that is repeated `freqi` times.
  - For example, `nums = [1,1,1,2,2,2,2,2]` is represented by the **run-length encoded** array `encoded = [ [1,3],[2,5] ]`. Another way to read this is "three `1`'s followed by five `2`'s".
- The **product** of two run-length encoded arrays `encoded1` and `encoded2` can be calculated using the following steps:
  1. **Expand** both `encoded1` and `encoded2` into the full arrays `nums1` and `nums2` respectively.
  2. Create a new array `prodNums` of length `nums1.length` and set `prodNums[i] = nums1[i] * nums2[i]`.
  3. **Compress** `prodNums` into a run-length encoded array and return it.
- You are given two **run-length encoded** arrays `encoded1` and `encoded2` representing full arrays `nums1` and `nums2` respectively. Both `nums1` and `nums2` have the **same length**. Each `encoded1[i] = [vali, freqi]` describes the `ith` segment of `nums1`, and each `encoded2[j] = [valj, freqj]` describes the `jth` segment of `nums2`.
- Return *the **product** of* `encoded1` *and* `encoded2`.
- **Note:** Compression should be done such that the run-length encoded array has the **minimum** possible length.

**Example 1**

```
Input: encoded1 = [ [1,3],[2,3] ], encoded2 = [ [6,3],[3,3] ]
Output: [ [6,6] ]
Explanation: encoded1 expands to [1,1,1,2,2,2] and encoded2 expands to [6,6,6,3,3,3].
prodNums = [6,6,6,6,6,6], which is compressed into the run-length encoded array [ [6,6] ].
```

**Example 2**

```
Input: encoded1 = [ [1,3],[2,1],[3,2] ], encoded2 = [ [2,3],[3,3] ]
Output: [ [2,3],[6,1],[9,2] ]
Explanation: encoded1 expands to [1,1,1,2,3,3] and encoded2 expands to [2,2,2,3,3,3].
prodNums = [2,2,2,6,9,9], which is compressed into the run-length encoded array [ [2,3],[6,1],[9,2] ].
```

## Method 1

```tex
【O(n) time | O(m) space】
```

```java
package Leetcode.TwoPointer;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2024/11/17
 */
public class ProductOfTwoRunLengthEncodedArrays {
    public static List<List<Integer>> findRLEArray(int[][] encoded1, int[][] encoded2) {
        List<List<Integer>> result = new ArrayList<>();
        int i = 0;      // Pointer for traversing encoded1
        int j = 0;      // Pointer for traversing encoded2
        int pos1 = 0;   // Remaining frequency count in current segment of encoded1
        int pos2 = 0;   // Remaining frequency count in current segment of encoded2

        // Process both arrays until we reach the end of either array
        while (i < encoded1.length && j < encoded2.length) {
            // Get current values from both arrays
            int val1 = encoded1[i][0];    // Current value from encoded1
            int val2 = encoded2[j][0];    // Current value from encoded2

            // If pos1 is 0, get new frequency from encoded1
            if(pos1 == 0){
                pos1 = encoded1[i][1];
            }

            // If pos2 is 0, get new frequency from encoded2
            if(pos2 == 0){
                pos2 = encoded2[j][1];
            }

            // Calculate the product of current values and the overlap length
            int product = val1 * val2;
            int overlap = Math.min(pos1, pos2);   // Get minimum frequency between two current segments

            // Handle the result:
            // If result is empty OR current product is different from the last product in result
            if(result.isEmpty() || result.get(result.size() - 1).get(0) != product){
                // Add new entry with current product and overlap frequency
                result.add(Arrays.asList(product, overlap));
            } else {
                // If current product equals last product in result, merge frequencies
                List<Integer> last = result.get(result.size() - 1);
                // Update frequency by adding the overlap
                // last.get(1) gets current frequency, overlap adds new frequency
                last.set(1, last.get(1) + overlap);
            }

            // Update remaining frequencies
            pos1 -= overlap;
            pos2 -= overlap;

            // Move pointer i if we've used up current segment in encoded1
            if(pos1 == 0){
                i++;
            }

            // Move pointer j if we've used up current segment in encoded2
            if(pos2 == 0){
                j++;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic case with same length arrays
        // Expected result: [ [6,6] ] (as 1×6=6 three times, and 2×3=6 three times)
        int[][] encoded1 = { {1,3}, {2,3} };
        int[][] encoded2 = { {6,3}, {3,3} };
        System.out.println("Test Case 1:");
        System.out.println("encoded1: " + Arrays.deepToString(encoded1));
        System.out.println("encoded2: " + Arrays.deepToString(encoded2));
        System.out.println("Result: " + findRLEArray(encoded1, encoded2));

        // Test Case 2: Different length arrays
        // Expected result: [ [2,3], [6,1], [9,2] ]
        int[][] encoded3 = { {1,3}, {2,1}, {3,2} };
        int[][] encoded4 = { {2,3}, {3,3} };
        System.out.println("\nTest Case 2:");
        System.out.println("encoded1: " + Arrays.deepToString(encoded3));
        System.out.println("encoded2: " + Arrays.deepToString(encoded4));
        System.out.println("Result: " + findRLEArray(encoded3, encoded4));

        // Test Case 3: Case with same products requiring merging
        // Expected result: [ [6,5], [4,1] ]
        int[][] encoded5 = { {2,3}, {3,2}, {4,1} };
        int[][] encoded6 = { {3,3}, {2,2}, {1,1} };
        System.out.println("\nTest Case 3:");
        System.out.println("encoded1: " + Arrays.deepToString(encoded5));
        System.out.println("encoded2: " + Arrays.deepToString(encoded6));
        System.out.println("Result: " + findRLEArray(encoded5, encoded6));
    }
}

```





