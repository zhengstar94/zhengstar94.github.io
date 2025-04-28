---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2070. Most Beautiful Item for Each Query"
date: "2025-03-09"
tags: Medium
categories:
  - "LeetCode BinarySearch"
---


- You are given a 2D integer array `items` where `items[i] = [pricei, beautyi]` denotes the **price** and **beauty** of an item respectively.
- You are also given a **0-indexed** integer array `queries`. For each `queries[j]`, you want to determine the **maximum beauty** of an item whose **price** is **less than or equal** to `queries[j]`. If no such item exists, then the answer to this query is `0`.
- Return *an array* `answer` *of the same length as* `queries` *where* `answer[j]` *is the answer to the* `jth` *query*.

**Example 1**

```
Input: items = [ [ 1,2],[3,2],[2,4],[5,6],[3,5 ] ], queries = [1,2,3,4,5,6]
Output: [2,4,5,5,6,6]
Explanation:
- For queries[0]=1, [1,2] is the only item which has price <= 1. Hence, the answer for this query is 2.
- For queries[1]=2, the items which can be considered are [1,2] and [2,4]. 
  The maximum beauty among them is 4.
- For queries[2]=3 and queries[3]=4, the items which can be considered are [1,2], [3,2], [2,4], and [3,5].
  The maximum beauty among them is 5.
- For queries[4]=5 and queries[5]=6, all items can be considered.
  Hence, the answer for them is the maximum beauty of all items, i.e., 6.
```

**Example 2**

```
Input: items = [ [ 1,2],[1,2],[1,3],[1,4 ] ], queries = [1]
Output: [4]
Explanation: 
The price of every item is equal to 1, so we choose the item with the maximum beauty 4. 
Note that multiple items can have the same price and/or beauty.  
```

**Example 3**

```
Input: items = [ [ 10,1000 ] ], queries = [5]
Output: [0]
Explanation:
No item has a price less than or equal to 5, so no item can be chosen.
Hence, the answer to the query is 0.
```

## Method 1

```tex
【O(nlogn + mlogm) time | O(m) space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/09
 */
public class MostBeautifulItemForEachQuery {
    
    public static int[] maximumBeauty(int[][] items, int[] queries) {
        // Sort items by price in ascending order
        Arrays.sort(items, (a, b) -> a[0] - b[0]);

        // Maintain prefix maximum beauty values
        // After this, items[i][1] will contain the maximum beauty value among all items with price <= items[i][0]
        int n = items.length;
        for (int i = 1; i < n; i++) {
            items[i][1] = Math.max(items[i][1], items[i-1][1]);
        }

        int m = queries.length;
        int[] answer = new int[m];

        // Process each query
        for (int i = 0; i < m; i++) {
            int query = queries[i];

            // Binary search to find the first position where price > query
            // This helps us find the range of items with price <= query
            int left = 0, right = n;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (items[mid][0] > query) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }

            // If we found valid items (left > 0), take the maximum beauty value at position (left-1)
            // Otherwise, return 0 as no items satisfy the price condition
            answer[i] = left > 0 ? items[left-1][1] : 0;
        }
        return answer;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Multiple items with different prices and beauty values
        int[][] items1 = { { 1,2},{3,2},{2,4},{5,6},{3,5 } };
        int[] queries1 = {1,2,3,4,5,6};
        System.out.println("Test Case 1 Result: " +
                Arrays.toString(maximumBeauty(items1, queries1)));
        // Expected output: [2,4,5,5,6,6]

        // Test Case 2: Items with same price but different beauty values
        int[][] items2 = { { 1,2},{1,2},{1,3},{1,4 } };
        int[] queries2 = {1};
        System.out.println("Test Case 2 Result: " +
                Arrays.toString(maximumBeauty(items2, queries2)));
        // Expected output: [4]

        // Test Case 3: Single item with price higher than query
        int[][] items3 = { { 10,1000 } };
        int[] queries3 = { 5 };
        System.out.println("Test Case 3 Result: " +
                Arrays.toString(maximumBeauty(items3, queries3)));
        // Expected output: [0]
    }
}

```





