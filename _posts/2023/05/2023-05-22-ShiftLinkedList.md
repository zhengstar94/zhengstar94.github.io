---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Shift Linked List"
date: "2023-05-22"
categories:
  - "Algorithm LinkedLists"
---

# Shift Linked List [Hard]

- Write a function that takes in the head of a Singly Linked List and an integer k, shifts the list in place (i.e., doesn't create a brand new list) by k positions, and returns its new head.

- Shifting a Linked List means moving its nodes forward or backward and wrapping them around the list where appropriate. For example, shifting a Linked List forward by one position would make its tail become the new head of the linked list.

- Whether nodes are moved forward or backward is determined by whether k is positive or negative.

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

- You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.



**Sample Input **

```
head = 0 -> 1 -> 2 -> 3 -> 4 -> 5 // the head node with value 0
k = 2
```

**Sample Output **

```
4 -> 5 -> 0 -> 1 -> 2 -> 3 // the new head node with value 4
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Putting aside the cases where k is a negative integer, where k is 0, or where k is larger than the length of the linked list, what does shifting the linked list by k positions entail exactly?</strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Putting aside the cases mentioned in Hint #1, shifting the linked list by k positions means moving the last k nodes in the linked list to the front of the linked list. What nodes in the linked list will you actually need to mutate? </strong></i>
</details>





<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>There are four nodes that really matter in this entire process: the original tail of the linked list, which will point to the original head of the linked list, the original head of the linked list, which will be pointed to by the original tail of the linked list, the new tail of the linked list, and the new head of the linked list. Note that the new head is the node that the new tail points to in the original, unshifted linked list. </strong></i>
</details>




<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>You can find the original tail of the linked list by simply traversing the linked list, starting at the original head of the linked list that you're given. You can find the new tail of the linked list by moving k positions from the original tail if k is positive (which means moving to the (lengthOfList - k)th position in the list, and you can easily count the length of the list as you traverse it to find its original tail). You can access the new head of the linked list once you've found its new tail, since it's the new tail's original next node. How will you handle the trickier values of k? </strong></i>
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
 * @date 2023/06/25
 */
public class ShiftLinkedList {

    public static class ListNode {
        int value;
        ListNode next;

        ListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static ListNode shiftLinkedList(ListNode head, int k) {
        // Handle special cases: empty linked list, only one node, 0 shift
        if (head == null || head.next == null || k == 0) {
            return head;
        }

        // Calculate the length of the linked list
        int length = 1;
        ListNode tail = head;
        while (tail.next != null) {
            tail = tail.next;
            length++;
        }

        //Calculate the actual number of times to move
        int shift = k % length;
        if (shift == 0) {
            return head;
        }

        // Calculate the position of the new tail node
        int newTailPosition = k > 0 ? length - shift : -shift;
        ListNode newTail = head;
        for (int i = 1; i < newTailPosition; i++) {
            newTail = newTail.next;
        }

        // Perform cyclic shift
        ListNode newHead = newTail.next;
        newTail.next = null;
        tail.next = head;

        return newHead;
    }

    public static void main(String[] args) {
        // Create the linked list: 0 -> 1 -> 2 -> 3 -> 4 -> 5
        ListNode head = new ListNode(0);
        head.next = new ListNode(1);
        head.next.next = new ListNode(2);
        head.next.next.next = new ListNode(3);
        head.next.next.next.next = new ListNode(4);
        head.next.next.next.next.next = new ListNode(5);

        int k = 2;

        ListNode newHead = shiftLinkedList(head, k);

        // Print the shifted linked list
        while (newHead != null) {
            System.out.print(newHead.value + " -> ");
            newHead = newHead.next;
        }
        System.out.println("null");
    }
}

```

