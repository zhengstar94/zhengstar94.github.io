---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "141.Linked List Cycle"
date: "2024-02-21"
categories:
  - "LeetCode LinkedList"
---


# LeetCode 141. Linked List Cycle 

- Given `head`, the head of a linked list, determine if the linked list has a cycle in it.
- There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.
- Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.

**Example 1**

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2**

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3**

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.LinkList;

/**
 * @author zhengstars
 * @date 2024/03/17
 */
public class LinkedListCycle {
    public static boolean hasCycle(ListNode head) {

        // If the head is null or the next node of head is null, it can't construct a cycle, return false
        if (head == null || head.next == null) {
            return false;
        }

        // Defining two pointers, 'slow' will move one step at a time, 'fast' will move two steps at a time
        ListNode slow = head;
        ListNode fast = head.next;

        // Use a while loop to move 'slow' and 'fast' until they meet each other
        while (slow != fast) {

            // If we have reached the end of the list, it means there is no cycle, return false
            if (fast == null || fast.next == null) {
                return false;
            }

            // Otherwise, move 'slow' one step and 'fast' two steps
            slow = slow.next;
            fast = fast.next.next;
        }

        // If 'slow' and 'fast' meet, it means there is a cycle in the list, return true
        return true;
    }

    // Define the main procedure for testing
    public static void main(String[] args) {

        // Create the first list, which has a cycle
        ListNode head1 = new ListNode(3);
        head1.next = new ListNode(2);
        head1.next.next = new ListNode(0);
        head1.next.next.next = new ListNode(-4);
        // Make the next node of the last node to be the second node (head1.next). Now, the list is a cycle
        head1.next.next.next.next = head1.next;
        // The method should return true because there is a cycle in the list
        System.out.println(hasCycle(head1)); // expect output: true

        // Create the second list, which has a cycle
        ListNode head2 = new ListNode(1);
        head2.next = new ListNode(2);
        // Make the next node of the last node to be the head node (head2). Now, the list is a cycle
        head2.next.next = head2;
        // The method should return true because there is a cycle in the list
        System.out.println(hasCycle(head2)); // expect output: true

        // Create the third list, which doesn't have a cycle
        ListNode head3 = new ListNode(1);
        // The method should return false because there is no cycle in the list
        System.out.println(hasCycle(head3)); // expect output: false
    }
}

```

