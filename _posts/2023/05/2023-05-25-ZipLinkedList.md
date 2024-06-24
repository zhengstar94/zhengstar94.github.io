---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Zip Linked List"
date: "2023-05-25"
categories:
  - "Algorithm LinkedLists"
---

# Zip Linked List [Very Hard]

- You're given the head of a Singly Linked List of arbitrary length k. Write a function that zips the Linked List in place (i.e., doesn't create a brand new list) and returns its head.

- A Linked List is zipped if its nodes are in the following order, where k is the length of the Linked List:

- ```
  1st node -> kth node -> 2nd node -> (k - 1)th node -> 3rd node -> (k - 2)th node -> ...
  ```

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

- You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.

**Sample Input **

```
linkedList = 1 -> 2 -> 3 -> 4 -> 5 -> 6 // the head node with value 1
```

**Sample Output **

```
1 -> 6 -> 2 -> 5 -> 3 -> 4 // the head node with value 1
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Try to imagine how you would solve this problem if you were given two distinct linked lists. For example, how would you zip the list 1 -> 2 -> 3 with the list 4 -> 5 to get 1 -> 5 -> 2 -> 4 -> 3? </strong></i>
</details>







<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>One of the most straightforward ways to solve this problem is to split the original linked list into two linked lists and to reverse the second linked list before interweaving it with the first one. Ultimately, you want the first node, then the kth node, then the second node, etc., so reversing the second linked list before interweaving it with the first one makes things simple. </strong></i>
</details>








<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>After you split the linked list into two halves and reverse the second half, you'll have something like 1 -> 2 -> 3 and 5 -> 4; at this point, you can simply add the first node of the reversed second half into the first half between 1 and 2 as in 1 -> 5 -> 2.... Simply continue this process until you've inserted all of the nodes from the reversed second half into the first.</strong></i>
</details>









<br>

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2023/06/30
 */
public class ZipLinkedList {
    public static class ListNode {
        int val;
        ListNode next;

        ListNode(int val) {
            this.val = val;
            this.next = null;
        }
    }

    public static ListNode zipLinkedList(ListNode head) {
        // When the length of the linked list is less than or equal to 2, it returns directly to the header node without compression.
        if (head == null || head.next == null || head.next.next == null) {
            return head;
        }

        // The fast and slow pointer finds the midpoint of the linked list
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // The second part is the header node of the linked list
        ListNode secondHalfHead = slow.next;
        // Cut off the linked list
        slow.next = null; 

        // Reverse the second part of the linked list
        secondHalfHead = reverseLinkedList(secondHalfHead);

        // Merge two linked lists
        ListNode p1 = head;
        ListNode p2 = secondHalfHead;
        while (p2 != null) {
            ListNode temp1 = p1.next;
            ListNode temp2 = p2.next;
            p1.next = p2;
            p2.next = temp1;
            p1 = temp1;
            p2 = temp2;
        }

        return head;
    }

    // Reverse linked list
    private static ListNode reverseLinkedList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        while (current != null) {
            ListNode next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }
        return prev;
    }

    public static void main(String[] args) {
        // Create a linked list: 1 -> 2 -> 3 -> 4 -> 5 -> 6
        ListNode head = new ListNode(1);
        ListNode node2 = new ListNode(2);
        ListNode node3 = new ListNode(3);
        ListNode node4 = new ListNode(4);
        ListNode node5 = new ListNode(5);
        ListNode node6 = new ListNode(6);
        head.next = node2;
        node2.next = node3;
        node3.next = node4;
        node4.next = node5;
        node5.next = node6;

        // Call function to compress linked list
        ListNode zippedHead = zipLinkedList(head);

        // Output compressed linked list
        printLinkedList(zippedHead);
    }

    private static void printLinkedList(ListNode head) {
        ListNode current = head;
        while (current != null) {
            System.out.print(current.val + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
}

```

