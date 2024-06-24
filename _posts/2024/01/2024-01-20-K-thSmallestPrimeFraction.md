---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "786. K-th Smallest Prime Fraction"
date: "2024-01-20"
categories:
  - "LeetCode Binary Search"
---

# LeetCode 786. K-th Smallest Prime Fraction [Hard]

- A sorted list `A` contains 1, plus some number of primes. Then, for every p < q in the list, we consider the fraction p/q.
- What is the `K`-th smallest fraction considered? Return your answer as an array of ints, where `answer[0] = p` and `answer[1] = q`.

**Example 1**

```
Examples:
Input: A = [1, 2, 3, 5], K = 3
Output: [2, 5]
Explanation:
The fractions to be considered in sorted order are:
1/5, 1/3, 2/5, 1/2, 3/5, 2/3.
The third fraction is 2/5.
 
Input: A = [1, 7], K = 1
Output: [1, 7]
```

## Method 1

```tex
【O(nlog(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/02/08
 */
public class KthSmallestPrimeFraction {

    public static int[] kthSmallestPrimeFraction(int[] array, int k) {
        // Define the number of elements in the array
        final int size = array.length;
        // Initialize the left boundary as 0 and the right boundary as 1.0 for binary search
        double left = 0;
        double right = 1.0;

        // The loop for binary search continues as long as the left boundary is less than the right boundary
        while (left < right) {
            // Calculate the current middle value, which is the current candidate fraction
            double middle = (left + right) / 2;
            // Initialize the maximum fraction as 0
            double maxFraction = 0.0;
            // Initialize the number of fractions that are less than the current middle value as 0
            int count = 0;
            // Initialize the indexes of the numerator and denominator corresponding to the maximum fraction in the array
            int indexP = 0;
            int indexQ = 0;

            // Outer loop traverses each possible numerator
            for (int i = 0, j = 1; i < size - 1; ++i) {
                // Inner loop traverses each possible denominator,
                // find the denominator that makes the numerator/denominator greater than the middle value for the first time
                while (j < size && array[i] > middle * array[j]) {
                    ++j;
                }

                // If all denominators make the current numerator/denominator equal to or less than the middle value,
                // exit the outer loop and start judging the next numerator
                if (size == j) {
                    break;
                }

                // Calculate the number of fractions that are less than the current middle value
                count += (size - j);
                // Calculate the current fraction
                final double fraction = (double) array[i] / array[j];

                // Check if the current fraction is the maximum fraction found so far,
                // if it is, update the maximum fraction and its corresponding indexes
                if (fraction > maxFraction) {
                    indexP = i;
                    indexQ = j;
                    maxFraction = fraction;
                }
            }

            // Check if the number of fractions less than the current middle value equals k
            if (count == k) {
                // If it is equal to k, it means that the kth smallest fraction is found, return its numerator and denominator
                return new int[] {array[indexP], array[indexQ]};
            } else if (count > k) {
                // If it is larger than k, it means that the current middle value is too large,
                // the right boundary needs to be adjusted for the binary search
                right = middle;
            } else {
                // If it is less than k, it means that the current middle value is too small,
                // the left boundary needs to be adjusted for the binary search
                left = middle;
            }
        }

        // If the kth smallest fraction cannot be found in all fractions, return an empty array
        return new int[] {};
    }

    public static void main(String[] args) {

        // Test case 1
        int[] arr1 = new int[]{1, 2, 3, 5};
        int k1 = 3;
        int[] result1 = kthSmallestPrimeFraction(arr1, k1);
        System.out.println("Test Case 1 Result：" + Arrays.toString(result1));   // Expected output：[2,5]

        // Test case 2
        int[] arr2 = new int[]{1, 7, 23, 29, 47};
        int k2 = 8;
        int[] result2 = kthSmallestPrimeFraction(arr2, k2);
        System.out.println("Test Case 2 Result：" + Arrays.toString(result2));   // Expected output：[7,23]

        // Test case 3：Array with larger elements
        int[] arr3 = new int[]{1, 13, 17, 59, 73, 89, 97};
        int k3 = 18;
        int[] result3 = kthSmallestPrimeFraction(arr3, k3);
        System.out.println("Test Case 3 Result：" + Arrays.toString(result3));   // Expected output：[13,59]

        // Test case 4：Return the smallest fraction in the array
        int[] arr4 = new int[]{1, 2, 3, 5};
        int k4 = 1;
        int[] result4 = kthSmallestPrimeFraction(arr4, k4);
        System.out.println("Test Case 4 Result：" + Arrays.toString(result4));   // Expected output：[1,5]

        // Test case 5：Return an empty array (can't find the kth smallest fraction)
        int[] arr5 = new int[]{1, 7, 23, 29, 47};
        int k5 = 20;
        int[] result5 = kthSmallestPrimeFraction(arr5, k5);
        System.out.println("Test Case 5 Result：" + Arrays.toString(result5));   // Expected output：[]
    }

}
```

