---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "143. Reorder List"
date: "2024-02-22"
categories:
  - "LeetCode LinkedList"
---

# LeetCode 143. Reorder List

- You are given the head of a singly linked-list. The list can be represented as:

  > ```
  > L0 → L1 → … → Ln - 1 → Ln
  > ```

- Reorder the list to be on the following form:

  > ```
  > L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
  > ```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1**

```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

**Example 2**

```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.LinkList;

/**
 * @author zhengstars
 * @date 2024/03/11
 */
public class ReorderList {
  public static void reorderList(ListNode head) {
    // if the list only has one or no elements, just return it
    if (head == null || head.next == null) {
      return;
    }

    // initialization of slow and fast pointers
    ListNode slow = head;
    ListNode fast = head;

    // move fast pointer twice as fast as the slow pointer
    // when fast reaches the end, slow is at the middle
    while (fast.next != null && fast.next.next != null) {
      slow = slow.next;
      fast = fast.next.next;
    }

    // The 'preMiddle' node is defined as the middle node of the original list.
    // slow is at the middle node of the original list now.
    ListNode preMiddle = slow;

    // Reverse the second half of the list using the method from the first piece of code.

    // Initialize the previous node as null.
    ListNode prev = null;
    // Set the current node as the first node of the second half of the list.
    ListNode curr = slow.next;

    // This loop is to reverse the second half of the list
    while (curr != null) {
      // temporarily store the next node of the current node.
      ListNode next = curr.next;
      // point the next of the current node to the previous node, realizing the reverse.
      curr.next = prev;
      // move the previous node to the current node, preparing for next reverse.
      prev = curr;
      // move the current node to the next node, preparing for next reverse.
      curr = next;
    }

    // Insert the reversed second half list to the middle of the original list.
    preMiddle.next = prev;

    // Reset the 'slow' node to the head of the original list, preparing for the merge.
    // Reset the 'fast' node to the head of the reversed second half list, preparing for the merge.
    slow = head;
    fast = preMiddle.next;

    // This loop is to merge the two halves of the list.
    while (slow != preMiddle) {

      // Before the merge, move the 'fast' node from the second half list.
      preMiddle.next = fast.next;

      // Connect the 'fast' node to be merged, to the rest of the merged list.
      fast.next = slow.next;

      // Insert the 'fast' node to the merged list after the 'slow' node.
      slow.next = fast;

      // Move the 'slow' node to the next of the 'fast', preparing for the next merge.
      slow = fast.next;

      // Point the 'fast' node to the next to be merged node from the second half list, preparing for the next merge.
      fast = preMiddle.next;

    }
  }

  public static void main(String[] args) {
    // create a list with 6 nodes
    ListNode head2 = new ListNode(1);
    head2.next = new ListNode(2);
    head2.next.next = new ListNode(3);
    head2.next.next.next = new ListNode(4);
    head2.next.next.next.next = new ListNode(5);
    head2.next.next.next.next.next = new ListNode(6);
    // reorder the list
    reorderList(head2);
    // print the list, expecting output: 1->6->2->5->3->4
  }
}




```

