---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "378. Kth Smallest Element in a Sorted Matrix"
date: "2024-01-17"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 378. Kth Smallest Element in a Sorted Matrix [Hard]

- Given a *n* x *n* matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.
- Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Example 1**

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,
 
return 13.
```

## Method 1

```tex
【O(nlogn*log(max – min))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/02/02
 */
public class KthSmallestElementInASortedMatrix {
    public static int kthSmallest(int[][] matrix, int k) {
        // Get the length of the matrix
        int n = matrix.length;
        // Initialize low as the minimum value in the matrix and high as the maximum value
        int low = matrix[0][0], high = matrix[n - 1][n - 1];
        // Perform binary search while low is less than high
        while (low < high) {
            // Get the mid-value of low and high
            int mid = low + ((high - low) >> 1);
            // Initialize a counter
            int count = 0, j = n - 1;
            // Go through each row of the matrix
            for (int i = 0; i < n; i++) {
                // Find the first number that is less than or equal to mid
                while (j >= 0 && matrix[i][j] > mid) {
                    j--;
                }
                // j + 1 is the number of elements in the current row that are not greater than mid. Add this to count.
                count += (j + 1);
            }
            // If there are k or more elements that are not greater than mid, then the k-th smallest number is in [low, mid]. Update high = mid.
            if (count < k) {
                low = mid + 1;
            } else {
                // Otherwise, k-th smallest number is in [mid+1, high]. Update low = mid + 1.
                high = mid;
            }
        }
        // Finally, when low and high are equal, that's the k-th smallest number we're looking for.
        return low;
    }

    public static void main(String[] args) {

        // Define a matrix for testing
        int[][] matrix = {
                {1,  5,  9},
                {10, 11, 13},
                {12, 13, 15}
        };

        // Specify the target k value
        int k = 8;

        // Print the result
        System.out.println("The kth smallest element in the matrix is: " + kthSmallest(matrix, k));
    }
}

```

