---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "19.Remove Nth Node From End of List"
date: "2024-02-23"
categories:
  - "LeetCode LinkedList"
---

# LeetCode 19. Remove Nth Node From End of List

- Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1**

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2**

```
Input: head = [1], n = 1
Output: []
```

**Example 3**

```
Input: head = [1,2], n = 1
Output: [1]
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
public class RemoveNthNodeFromEndOfList {
    public static  ListNode removeNthFromEnd(ListNode head, int n) {
        // Creating a dummy node and let its next be the head node. 
        // This can make the process easier when the head node needs to be removed.
        ListNode dummy = new ListNode(0, head);

        // Get the length of the linked list
        int length = getLength(head);

        // Initialise a cursor pointing at the dummy node
        ListNode cur = dummy;

        // Move the cursor to the node before the node to be removed
        for (int i = 0; i < length - n; i++) {
            cur = cur.next;
        }

        // Remove the nth node from the end
        cur.next = cur.next.next;

        // Return the head node of the modified list
        return dummy.next;
    }

    private static int getLength(ListNode head) {
        int length = 0;

        // Traverse the list to accumulate the length
        while (head != null){
            length++;
            head = head.next;
        }

        return length;
    }

    public static void main(String[] args){
        // Example 1
        ListNode head1 = new ListNode(1);
        head1.next = new ListNode(2);
        head1.next.next = new ListNode(3);
        head1.next.next.next = new ListNode(4);
        head1.next.next.next.next = new ListNode(5);
        System.out.println(removeNthFromEnd(head1, 2)); // expect output: [1,2,3,5]

        // Example 2
        ListNode head2 = new ListNode(1);
        System.out.println(removeNthFromEnd(head2, 1)); // expect output: []

        // Example 3
        ListNode head3 = new ListNode(1);
        head3.next = new ListNode(2);
        System.out.println(removeNthFromEnd(head3, 1)); // expect output: [1]
    }
}

```

