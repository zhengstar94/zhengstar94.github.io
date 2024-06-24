---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Merge Linked Lists"
date: "2023-05-21"
categories:
  - "Algorithm LinkedLists"
---

# Merge Linked Lists [Hard]

- Write a function that takes in the heads of two Singly Linked Lists that are in sorted order, respectively. The function should merge the lists in place (i.e., it shouldn't create a brand new list) and return the head of the merged list; the merged list should be in sorted order.

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

- You can assume that the input linked lists will always have at least one node; in other words, the heads will never be None / null.



**Sample Input **

```
headOne = 2 -> 6 -> 7 -> 8 // the head node with value 2
headTwo = 1 -> 3 -> 4 -> 5 -> 9 -> 10 // the head node with value 1
```

**Sample Output **

```
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 // the new head node with value 1
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>You can iterate through the Linked Lists from head to tail and merge them along the way by inserting nodes from the second Linked List into the first Linked List. </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>You'll need to manipulate three nodes at once at every step. </strong></i>
</details>




<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>At every step, you'll need to have three variables (p1, p2, and p1Prev) pointing to the current node in the first Linked List (p1), the current node in the second Linked List (p2), and the previous node in the first Linked List (p1Prev). If the value of p1 is smaller than the value of p2, then you can just "move forward" in the first Linked List by moving p1 and p1Prev forward by one position (p1Prev becomes p1 and p1 becomes p1.next). If the value of p1 is greater than the value of p2, then you need to insert p2 before p1. You'll have to first make p1Prev point to p2, then make p2 point to p1, all the while not losing track of p2's "next" node, which you'll need to move to right after. You'll also have to handle edge cases when you're dealing with head nodes or tail nodes. </strong></i>
</details>




<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>You can implement this algorithm both iteratively and recursively following nearly identical logic. </strong></i>
</details>




<br>

## Method 1

```tex
【O(n + m)time∣O(1)space】
```

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2023/06/24
 */
public class MergeLinkedLists {
    public static class ListNode {
        int value;
        ListNode next;

        public ListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static ListNode mergeLinkedLists(ListNode headOne, ListNode headTwo) {
        // Virtual head node
        ListNode dummyHead = new ListNode(-1); 
        ListNode current = dummyHead;

        // Compare the node values of the two linked lists and link the nodes with smaller values to the new linked list in order
        while (headOne != null && headTwo != null) {
            if (headOne.value <= headTwo.value) {
                current.next = headOne;
                headOne = headOne.next;
            } else {
                current.next = headTwo;
                headTwo = headTwo.next;
            }
            current = current.next;
        }

        // Link the remaining nodes to the new linked list
        if (headOne != null) {
            current.next = headOne;
        } else {
            current.next = headTwo;
        }

        return dummyHead.next;
    }

    public static void main(String[] args) {
        // Construction list 1
        ListNode headOne = new ListNode(2);
        ListNode node1 = new ListNode(6);
        ListNode node2 = new ListNode(7);
        ListNode node3 = new ListNode(8);

        headOne.next = node1;
        node1.next = node2;
        node2.next = node3;

        // Construction list 2
        ListNode headTwo = new ListNode(1);
        ListNode node4 = new ListNode(3);
        ListNode node5 = new ListNode(4);
        ListNode node6 = new ListNode(5);
        ListNode node7 = new ListNode(9);
        ListNode node8 = new ListNode(10);

        headTwo.next = node4;
        node4.next = node5;
        node5.next = node6;
        node6.next = node7;
        node7.next = node8;

        // Merge linked list
        ListNode mergedHead = mergeLinkedLists(headOne, headTwo);
        
        ListNode currentNode = mergedHead;
        while (currentNode != null) {
            System.out.print(currentNode.value + " -> ");
            currentNode = currentNode.next;
        }
        System.out.print("null");
    }
}
```

