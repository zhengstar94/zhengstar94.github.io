---
toc:
beginning: true
giscus_comments: true
layout: post
title: "1290. Convert Binary Number in a Linked List to Integer"
date: "2025-07-14"
tags: Easy
categories:
- "LeetCode LinkedList"
---

- Given `head` which is a reference node to a singly-linked list. The value of each node in the linked list is either `0` or `1`. The linked list holds the binary representation of a number.
- Return the *decimal value* of the number in the linked list.
- The **most significant bit** is at the head of the linked list.

**Example 1**

```
Input: head = [1,0,1]
Output: 5
Explanation: (101) in base 2 = (5) in base 10
```

**Example 2**

```
Input: head = [0]
Output: 0
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.LinkList;

/**
 * @Author zhengxingxing
 * @Date 2025/07/14
 */

public class ConvertBinaryNumberInALinkedListToInteger {

    public static int getDecimalValue(ListNode head) {
        int res = 0; // This variable will store the final decimal result.

        // Traverse the linked list from head to tail.
        while (head != null) {
            // Shift the current result left by 1 bit (multiply by 2).
            // This makes space for the next bit at the least significant position.
            // Then add the current node's value (0 or 1) to the result.
            res = res * 2 + head.val;

            // Move to the next node in the list.
            head = head.next;
        }

        // After traversing all nodes, 'res' contains the decimal value.
        return res;
    }

    public static void main(String[] args) {
        // Example 1: Linked list [1,0,1] represents binary 101, which is 5 in decimal.
        ListNode head1 = new ListNode(1);
        head1.next = new ListNode(0);
        head1.next.next = new ListNode(1);
        System.out.println(getDecimalValue(head1)); // Output: 5

        // Example 2: Linked list [0] represents binary 0, which is 0 in decimal.
        ListNode head2 = new ListNode(0);
        System.out.println(getDecimalValue(head2)); // Output: 0
    }
}

```





