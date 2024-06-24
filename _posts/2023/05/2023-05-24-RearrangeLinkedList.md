---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Rearrange Linked List"
date: "2023-05-24"
categories:
  - "Algorithm LinkedLists"
---

# Rearrange Linked List [Very Hard]

- Write a function that takes in the head of a Singly Linked List and an integer k, rearranges the list in place (i.e., doesn't create a brand new list) around nodes with value k, and returns its new head.
- Rearranging a Linked List around nodes with value k means moving all nodes with a value smaller than k before all nodes with value k and moving all nodes with a value greater than k after all nodes with value k.
- All moved nodes should maintain their original relative ordering if possible.
- Note that the linked list should be rearranged even if it doesn't have any nodes with value k.
- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.
- You can assume that the input Linked List will always have at least one node; in other words, the head will never be None / null.

**Sample Input **

```
head = 3 -> 0 -> 5 -> 2 -> 1 -> 4 // the head node with value 3
k = 3
```

**Sample Output **

```
0 -> 2 -> 1 -> 3 -> 5 -> 4 // the new head node with value 0
// Note that the nodes with values 0, 2, and 1 have
// maintained their original relative ordering, and
// so have the nodes with values 5 and 4.
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>The final linked list that you have to return essentially consists of three linked lists attached to one another: one with nodes whose values are smaller than k, one with nodes whose values are equal to k, and one with nodes whose values are greater than k. </strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Iterate through the linked list once, build the three linked lists mentioned in Hint #1 as you go, and finally connect these three linked lists to form the rearranged list. </strong></i>
</details>






<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>To build the three linked lists mentioned in Hints #1 and #2, you'll have to keep track of their heads and tails and update the appropriate linked list's tail with each node that you traverse as you iterate through the main linked list. You can determine which linked list is the relevant one by simply comparing the value of the node that you're traversing to k. </strong></i>
</details>





<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>Connecting the three linked lists mentioned in the previous Hint won't be as simple as it sounds, mainly because one or two of the linked lists might actually be empty, depending on the various nodes' values and the value of k. </strong></i>
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
 * @date 2023/06/26
 */
public class RearrangeLinkedList {
    public static class ListNode {
        int value;
        ListNode next;

        ListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static ListNode rearrangeLinkedList(ListNode head, int k) {
        ListNode smallerHead = null;
        ListNode smallerTail = null;
        ListNode equalHead = null;
        ListNode equalTail = null;
        ListNode greaterHead = null;
        ListNode greaterTail = null;

        ListNode current = head;
        while (current != null) {
            if (current.value < k) {
                if (smallerHead == null) {
                    smallerHead = current;
                    smallerTail = current;
                } else {
                    smallerTail.next = current;
                    smallerTail = smallerTail.next;
                }
            } else if (current.value == k) {
                if (equalHead == null) {
                    equalHead = current;
                    equalTail = current;
                } else {
                    equalTail.next = current;
                    equalTail = equalTail.next;
                }
            } else {
                if (greaterHead == null) {
                    greaterHead = current;
                    greaterTail = current;
                } else {
                    greaterTail.next = current;
                    greaterTail = greaterTail.next;
                }
            }
            current = current.next;
        }

        if (greaterTail != null) {
            greaterTail.next = null;
        }

        if (smallerHead == null) {
            if (equalHead == null) {
                return greaterHead;
            } else {
                equalTail.next = greaterHead;
                return equalHead;
            }
        } else {
            if (equalHead == null) {
                smallerTail.next = greaterHead;
                return smallerHead;
            } else {
                smallerTail.next = equalHead;
                equalTail.next = greaterHead;
                return smallerHead;
            }
        }
    }

    public static void main(String[] args) {
        // Create the linked list: 3 -> 0 -> 5 -> 2 -> 1 -> 4
        ListNode head = new ListNode(3);
        head.next = new ListNode(0);
        head.next.next = new ListNode(5);
        head.next.next.next = new ListNode(2);
        head.next.next.next.next = new ListNode(1);
        head.next.next.next.next.next = new ListNode(4);

        int k = 3;

        ListNode newHead = rearrangeLinkedList(head, k);

        // Print the rearranged linked list
        while (newHead != null) {
            System.out.print(newHead.value + " -> ");
            newHead = newHead.next;
        }
        System.out.println("null");
    }
}

```

