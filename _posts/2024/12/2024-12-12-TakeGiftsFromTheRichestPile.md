---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2558. Take Gifts From the Richest Pile"
date: "2024-12-12"
tags: Easy
categories:
  - "LeetCode Greedy"
---

- You are given an integer array `gifts` denoting the number of gifts in various piles. Every second, you do the following:
  - Choose the pile with the maximum number of gifts.
  - If there is more than one pile with the maximum number of gifts, choose any.
  - Leave behind the floor of the square root of the number of gifts in the pile. Take the rest of the gifts.
- Return *the number of gifts remaining after* `k` *seconds.*

**Example 1**

```
Input: gifts = [25,64,9,4,100], k = 4
Output: 29
Explanation: 
The gifts are taken in the following way:
- In the first second, the last pile is chosen and 10 gifts are left behind.
- Then the second pile is chosen and 8 gifts are left behind.
- After that the first pile is chosen and 5 gifts are left behind.
- Finally, the last pile is chosen again and 3 gifts are left behind.
The final remaining gifts are [5,8,9,4,3], so the total number of gifts remaining is 29.
```

**Example 2**

```
Input: gifts = [1,1,1,1], k = 4
Output: 4
Explanation: 
In this case, regardless which pile you choose, you have to leave behind 1 gift in each pile. 
That is, you can't take any pile with you. 
So, the total gifts remaining are 4.
```

## Method 1

```tex
【O(k * log(n)) time | O(n) space】
```

```java
package Leetcode.Greedy;

import java.util.PriorityQueue;

/**
 * @author zhengxingxing
 * @date 2024/12/12
 */
public class TakeGiftsFromTheRichestPile {
    public static long pickGifts(int[] gifts, int k) {
        // Create a max heap (priority queue with descending order)
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a,b) -> b - a);

        // Add all gift quantities to the max heap
        for (int gift: gifts) {
            maxHeap.add(gift);
        }

        // Perform k operations
        while (k-- > 0){
            // Take the largest gift pile
            int gift = maxHeap.poll();

            // Calculate square root and round down
            gift = (int)Math.sqrt(gift);

            // Add modified gift pile back to the heap
            maxHeap.add(gift);
        }

        // Calculate total remaining gifts
        long ans = 0;
        while (!maxHeap.isEmpty()){
            ans += maxHeap.poll();
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] gifts1 = {25, 64, 9, 4, 100};
        int k1 = 4;
        System.out.println("Test case 1 result: " + pickGifts(gifts1, k1)); // Expected output: 29

        // Test case 2
        int[] gifts2 = {1, 1, 1, 1};
        int k2 = 4;
        System.out.println("Test case 2 result: " + pickGifts(gifts2, k2)); // Expected output: 4
    }
}

```





