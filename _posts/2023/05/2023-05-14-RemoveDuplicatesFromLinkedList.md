---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Remove Duplicates From Linked List"
date: "2023-05-14"
categories:
  - "Algorithm LinkedLists"
---

# Remove Duplicates From Linked List [Easy]

- You're given the head of a Singly Linked List whose nodes are in sorted order with respect to their values. Write a function that returns a modified version of the Linked List that doesn't contain any nodes with duplicate values. The Linked List should be modified in place (i.e., you shouldn't create a brand new list), and the modified Linked List should still have its nodes sorted with respect to their values.

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.



**Sample Input **

```
linkedList = 1 -> 1 -> 3 -> 4 -> 4 -> 4 -> 5 -> 6 -> 6 // the head node with value 1
```

**Sample Output **

```
1 -> 3 -> 4 -> 5 -> 6 // the head node with value 1
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>The brute-force approach to this problem is to use a hash table or a set to keep track of all node values that exist while traversing the linked list and to simply remove nodes that have a value that already exists. This approach works, but can you solve this problem without using an auxiliary data structure? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>What does the fact that the nodes are sorted tell you about the location of all duplicate nodes? How can you use this fact to solve this problem with constant space? </strong></i>
</details>




<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Since the linked list's nodes are sorted, you can loop through them and, at each iteration, simply remove all successive nodes that have the same value as the current node. For each node, change its next pointer to the next node in the linked list that has a different value. This will remove all duplicate-value nodes. </strong></i>
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
 * @date 2023/06/11
 */
public class RemoveDuplicatesFromLinkedList {
    public static class ListNode {
        int val;
        ListNode next;

        ListNode(int val) {
            this.val = val;
            this.next = null;
        }
    }


    public static ListNode removeDuplicates(ListNode head) {
        ListNode current = head;

        // Traverse the linked list
        while (current != null && current.next != null) {
            // If current node value is equal to the next node value,
            // it means there is a duplicate node. We remove the duplicate node
            // by updating the next pointer of the current node to skip the duplicate node.
            if (current.val == current.next.val) {
                current.next = current.next.next;
            } else {
                // If the current node value is different from the next node value,
                // it means the current node is not a duplicate. Move the current pointer
                // to the next node and continue the traversal.
                current = current.next;
            }
        }

        // Return the modified linked list
        return head;
    }

    public static void main(String[] args) {

        ListNode node1 = new ListNode(1);
        ListNode node2 = new ListNode(1);
        ListNode node3 = new ListNode(3);
        ListNode node4 = new ListNode(4);
        ListNode node5 = new ListNode(4);
        ListNode node6 = new ListNode(4);
        ListNode node7 = new ListNode(5);
        ListNode node8 = new ListNode(6);
        ListNode node9 = new ListNode(6);

        node1.next = node2;
        node2.next = node3;
        node3.next = node4;
        node4.next = node5;
        node5.next = node6;
        node6.next = node7;
        node7.next = node8;
        node8.next = node9;

        ListNode result = removeDuplicates(node1);
        while (result != null) {
            System.out.print(result.val + " -> ");
            result = result.next;
        }
    }
}

```

