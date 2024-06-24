---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "206.Reverse Linked List"
date: "2024-02-19"
categories:
  - "LeetCode LinkedList"
---

# LeetCode 206. Reverse Linked List 

- Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

**Example 1**

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2**

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3**

```
Input: head = []
Output: []
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.LinkList;

/**
 * @author zhengstars
 * @date 2024/03/07
 */
public class ReverseLinkedList {
    
    public static ListNode reverseList(ListNode head) {
        // prev points to the node before curr
        ListNode prev = null;
        // curr points to the current node we're at
        ListNode curr = head;
        // While there is still unprocessed node
        while (curr != null) {
            // Store the next node of curr before changing curr.next
            ListNode nextTemp = curr.next;
            // Reverse the direction of the link
            curr.next = prev;
            // Move on to the next node
            prev = curr;
            curr = nextTemp;
        }
        // At the end of the list, prev will be the new head
        return prev;
    }

    public static void main(String[] args) {
        // Test Case 1
        // Create a list with nodes [1->2->3->4->5]
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        head.next.next.next.next = new ListNode(5);
        // Reverse the list and store the new head
        ListNode reversedList = reverseList(head);
        // Print the reversed list
        while(reversedList != null) {
            System.out.print(reversedList.val + " ");
            reversedList = reversedList.next;
        }
        System.out.println();
    }
}

/**
 * Definition for singly-linked list node.
 */
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        val = x;
    }
}
```

