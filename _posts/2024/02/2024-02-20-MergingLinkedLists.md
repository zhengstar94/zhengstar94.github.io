---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "21.Merging Linked Lists"
date: "2024-02-20"
categories:
  - "LeetCode LinkedList"
---


# LeetCode 21. Merging Linked Lists 

- You are given the heads of two sorted linked lists `list1` and `list2`.

- Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

- Return *the head of the merged linked list*.



**Example 1: **

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2: **

```
Input: list1 = [], list2 = []
Output: []
```

**Example 3: **

```
Input: list1 = [], list2 = [0]
Output: [0]
```

<br>

## Method 1

```tex
【O(n + m)time∣O(1)space】
```

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2023/06/16
 */
public class MergingLinkedLists {
    public static class ListNode {
        int val;
        ListNode next;

        ListNode(int val) {
            this.val = val;
        }
    }

    public static ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Create the head node of the merged list
        ListNode mergedHead = new ListNode(0);
        // Current node pointer
        ListNode current = mergedHead;

        ListNode p1 = list1;
        ListNode p2 = list2;

        while (p1 != null && p2 != null) {
            if (p1.val <= p2.val) {
                // Add p1 node to mergedHead
                current.next = p1;
                // Move p1 pointer
                p1 = p1.next;
            } else {
                // Add p2 node to mergedHead
                current.next = p2;
                // Move p2 pointer
                p2 = p2.next;
            }
            // Move current node pointer
            current = current.next;
        }

        // Add the remaining part to the end of mergedHead
        if (p1 != null) {
            current.next = p1;
        }
        if (p2 != null) {
            current.next = p2;
        }

        // Return the head node of the merged list
        return mergedHead.next;
    }

    public static void main(String[] args) {
        // list1: 1 -> 2 -> 4
        ListNode node1 = new ListNode(1);
        ListNode node2 = new ListNode(2);
        ListNode node4 = new ListNode(4);
        node1.next = node2;
        node2.next = node4;

        // list2: 1 -> 3 -> 4
        ListNode node1_2 = new ListNode(1);
        ListNode node3 = new ListNode(3);
        ListNode node4_2 = new ListNode(4);
        node1_2.next = node3;
        node3.next = node4_2;

        ListNode mergedList = mergeTwoLists(node1, node1_2);

        while (mergedList != null) {
            System.out.print(mergedList.val + " ");
            mergedList = mergedList.next;
        }
    }
}
```

