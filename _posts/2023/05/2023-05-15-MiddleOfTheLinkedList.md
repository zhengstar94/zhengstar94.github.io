---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Middle of the Linked List"
date: "2023-05-15"
categories:
  - "Algorithm LinkedLists"
---

# Middle of the Linked List [Easy]

- Given the `head` of a singly linked list, return *the middle node of the linked list*.

- If there are two middle nodes, return **the second middle** node.



**Sample Input **

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

**Sample Output **

```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

<br>

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2023/06/11
 */
public class MiddleoftheLinkedList {
    public static class ListNode {
        int val;
        ListNode next;

        ListNode(int val) {
            this.val = val;
            this.next = null;
        }
    }

    public static ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        // Move the fast pointer two steps ahead and the slow pointer one step ahead
        // When the fast pointer reaches the end of the linked list, the slow pointer will be at the middle node
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Return the middle node
        return slow;
    }

    public static void main(String[] args) {
        ListNode node1 = new ListNode(1);
        ListNode node2 = new ListNode(2);
        ListNode node3 = new ListNode(3);
        ListNode node4 = new ListNode(4);
        ListNode node5 = new ListNode(5);
        ListNode node6 = new ListNode(6);

        node1.next = node2;
        node2.next = node3;
        node3.next = node4;
        node4.next = node5;
        node5.next = node6;

        ListNode result = middleNode(node1);

        while (result != null) {
            System.out.print(result.val + " -> ");
            result = result.next;
        }
        System.out.println("null");
    }
}

```

