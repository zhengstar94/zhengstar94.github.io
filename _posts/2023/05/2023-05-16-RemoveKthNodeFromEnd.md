---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Remove Kth Node From End"
date: "2023-05-16"
categories:
  - "Algorithm LinkedLists"
---

# Remove Kth Node From End [Medium]

- Write a function that takes in the head of a Singly Linked List and an integer k and removes the kth node from the end of the list.

- The removal should be done in place, meaning that the original data structure should be mutated (no new structure should be created).

- Furthermore, the input head of the linked list should remain the head of the linked list after the removal is done, even if the head is the node that's supposed to be removed. In other words, if the head is the node that's supposed to be removed, your function should simply mutate its value and next pointer.

- Note that your function doesn't need to return anything.

- You can assume that the input Linked List will always have at least two nodes and, more specifically, at least k nodes.

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.



**Sample Input **

```
head = 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 // the head node with value 0
k = 4
```

**Sample Output **

```
// No output required.
// The 4th node from the end of the list (the node with value 6) is removed.
0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 7 -> 8 -> 9
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Since you are given a Singly Linked List, you do not have access to any of the list's nodes' previous nodes. Thus, traversing the entire list and then counting k nodes back isn't an option. Is there a way for you to traverse the entire list and to know which node is the kth node from the end by the time you reach the final node in the list? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Can you accomplish the task mentioned in Hint #1 by traversing the list all the while keeping track of two nodes at a time. How could this work? </strong></i>
</details>




<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Initialize two variables pointing to the first node in the list. Traverse k nodes in the list, updating the second variable at every node (that is, take k steps with the second variable). Then, traverse the remainder of the list, this time updating both the second and the first variables (that is take as many steps with the first variable as the number of steps between the kth node from the start and the end of the list). Once you reach the end of the list, the first variable should point to the kth node from the end. </strong></i>
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
 * @date 2023/06/13
 */
public class RemoveKthNodeFromEnd {

    public static class LinkedListNode {
        int value;
        LinkedListNode next;

        LinkedListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static void removeKthNodeFromEnd(LinkedListNode head, int k) {
        LinkedListNode slow = head;
        LinkedListNode fast = head;

        // Let the fast pointer move forward k nodes first
        for (int i = 0; i < k; i++) {
            fast = fast.next;
        }

        // If the fast pointer is empty at this time, the header node is to be deleted
        if (fast == null) {
            head.value = head.next.value;
            head.next = head.next.next;
            return;
        }

        // Move the slow and fast pointers forward at the same time until the fast pointer reaches the end of the list
        while (fast.next != null) {
            slow = slow.next;
            fast = fast.next;
        }

        // At this point, the slow pointer points to the previous node to delete the node.
        slow.next = slow.next.next;
        System.out.println("1");
    }

    public static void main(String[] args) {
        LinkedListNode head = new LinkedListNode(0);
        LinkedListNode node1 = new LinkedListNode(1);
        LinkedListNode node2 = new LinkedListNode(2);
        LinkedListNode node3 = new LinkedListNode(3);
        LinkedListNode node4 = new LinkedListNode(4);
        LinkedListNode node5 = new LinkedListNode(5);
        LinkedListNode node6 = new LinkedListNode(6);
        LinkedListNode node7 = new LinkedListNode(7);
        LinkedListNode node8 = new LinkedListNode(8);
        LinkedListNode node9 = new LinkedListNode(9);

        head.next = node1;
        node1.next = node2;
        node2.next = node3;
        node3.next = node4;
        node4.next = node5;
        node5.next = node6;
        node6.next = node7;
        node7.next = node8;
        node8.next = node9;

        int k = 4;

        removeKthNodeFromEnd(head, k);

        // Print deleted linked list
        printLinkedList(head);
    }

    public static void printLinkedList(LinkedListNode head) {
        LinkedListNode current = head;
        while (current != null) {
            System.out.print(current.value + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
}
```

