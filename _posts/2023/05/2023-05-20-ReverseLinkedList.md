---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Reverse Linked List"
date: "2023-05-20"
categories:
  - "Algorithm LinkedLists"
---

# Reverse Linked List [Hard]

- Write a function that takes in the head of a Singly Linked List, reverses the list in place (i.e., doesn't create a brand new list), and returns its new head.

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

- You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.



**Sample Input **

```
head = 0 -> 1 -> 2 -> 3 -> 4 -> 5 // the head node with value 0
```

**Sample Output **

```
5 -> 4 -> 3 -> 2 -> 1 -> 0 // the new head node with value 5
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>You can iterate through the Linked List from head to tail and reverse it in place along the way. </strong></i>
</details>







<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>You'll need to manipulate three nodes at once at every step. </strong></i>
</details>







<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Imagine you have three variables pointing to three consecutive nodes in a Linked List. Start by setting the "next" property of the second node to the first node. Then, set the first variable to the second node, and set the second variable to the third node. Finally, set the third variable to the second variable's "next" property (at this point, the second variable is the original third node). Repeat this process until you're at the tail of the Linked List. </strong></i>
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
 * @date 2023/06/24
 */
public class ReverseLinkedList {
    public static class ListNode {
        int value;
        ListNode next;

        public ListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static ListNode reverseLinkedList(ListNode head) {
        // Used to save the previous node of the current node
        ListNode prev = null;
        // Current node
        ListNode curr = head;
        // Used to save the latter node of the current node
        ListNode next = null;

        while (curr != null) {
            // 保存当前节点的后一个节点
            next = curr.next;
            // Reverse the pointer of the current node to the previous node
            curr.next = prev;

            // Move the pointer to the right
            prev = curr;
            curr = next;
        }

        // Return to the new header node
        return prev;
    }

    public static void main(String[] args) {
        // Construct linked list
        ListNode head = new ListNode(0);
        ListNode node1 = new ListNode(1);
        ListNode node2 = new ListNode(2);
        ListNode node3 = new ListNode(3);
        ListNode node4 = new ListNode(4);
        ListNode node5 = new ListNode(5);

        head.next = node1;
        node1.next = node2;
        node2.next = node3;
        node3.next = node4;
        node4.next = node5;

        // Reverse linked list
        ListNode newHead = reverseLinkedList(head);

        ListNode currentNode = newHead;
        while (currentNode != null) {
            System.out.print(currentNode.value + " -> ");
            currentNode = currentNode.next;
        }
        System.out.print("null");
    }
}

```

