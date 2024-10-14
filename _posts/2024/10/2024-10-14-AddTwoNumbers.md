---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2. Add Two Numbers"
date: "2024-10-14"
categories:
  - "LeetCode LinkedList"
---

- You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
- You may assume the two numbers do not contain any leading zero, except the number 0 itself.


**Example 1**

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

**Example 2**

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3**

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

## Method 1

```tex
【O(max(n, m)) time | O(max(n, m)) space】
```

```java
package Leetcode.LinkList;

/**
 * @author zhengstars
 * @date 2024/10/14
 */
public class AddTwoNumbers {
    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // Create a dummy head node to simplify the addition process
        ListNode dummyHead = new ListNode(0);
        ListNode current = dummyHead;

        // Initialize carry to 0
        int carry = 0;

        // Continue loop while there are digits in either list or there's a carry
        while (l1 != null || l2 != null) {
            // Get the value of the current digit from l1, or 0 if l1 is null
            int x = (l1 != null) ? l1.val : 0;
            // Get the value of the current digit from l2, or 0 if l2 is null
            int y = (l2 != null) ? l2.val : 0;

            // Calculate the sum of current digits and the carry from the previous addition
            int sum = carry + x + y;

            // Calculate the new carry for the next iteration
            // If sum >= 10, carry will be 1; otherwise, it will be 0
            carry = sum / 10;

            // Create a new node with the ones digit of the sum (sum % 10)
            // This effectively keeps only the ones digit and "carries" the tens digit
            current.next = new ListNode(sum % 10);
            current = current.next;

            // Move to the next digits in the input lists, if available
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }

        // After the main loop, check if there's still a carry left
        // This handles cases where the sum results in an additional digit
        // For example: 999 + 1 = 1000, so we need to add the final '1'
        if (carry > 0) {
            current.next = new ListNode(carry);
        }

        // Return the result, skipping the dummy head
        return dummyHead.next;
    }

    public static void main(String[] args) {
        // Create the first linked list: 2 -> 4 -> 3 (representing 342)
        ListNode l1 = new ListNode(2);
        l1.next = new ListNode(4);
        l1.next.next = new ListNode(3);

        // Create the second linked list: 5 -> 6 -> 4 (representing 465)
        ListNode l2 = new ListNode(5);
        l2.next = new ListNode(6);
        l2.next.next = new ListNode(4);

        // Add the two numbers
        ListNode result = addTwoNumbers(l1, l2);

        // Print the result
        System.out.print("Result: ");
        while (result != null) {
            System.out.print(result.val + " ");
            result = result.next;
        }
        // Expected output: 7 0 8 (representing 807, which is 342 + 465)
    }
}

```
