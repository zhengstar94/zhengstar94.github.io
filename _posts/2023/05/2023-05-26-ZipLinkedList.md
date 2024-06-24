---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Linked List Palindrome"
date: "2023-05-26"
categories:
  - "Algorithm LinkedLists"
---

# Linked List Palindrome [Very Hard]

- Write a function that takes in the head of a Singly Linked List and returns a boolean representing whether the linked list's nodes form a palindrome. Your function shouldn't make use of any auxiliary data structure.

- A palindrome is usually defined as a string that's written the same forward and backward. For a linked list's nodes to form a palindrome, their values must be the same when read from left to right and from right to left. Note that single-character strings are palindromes, which means that single-node linked lists form palindromes.

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list.

- You can assume that the input linked list will always have at least one node; in other words, the head will never be None / null.

  ## 

**Sample Input **

```
head = 0 -> 1 -> 2 -> 2 -> 1 -> 0 // the head node with value 0
```

**Sample Output **

```
true
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>Think about comparing two nodes at a time. To determine if the linked list's nodes form a palindrome, which two nodes should we compare? </strong></i>
</details>






<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Following Hint #1, to determine if the linked list's nodes form a palindrome, we'll want to compare the first and last node, the second and second-to-last node, the third and third-to-last node, etc.. How can we compare all of these nodes recursively? </strong></i>
</details>







<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Putting aside the recursive solution hinted at in Hint #2, we can solve this problem iteratively and with no auxiliary space if we know how to reverse a linked list. How can reversing the linked list (or part of it) help us solve this problem? </strong></i>
</details>






<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>Try reversing the second half of the linked list and then comparing nodes in the first half and in the reversed second half by simply iterating through both halves at the same time. You'll have to figure out where the second half of the linked list begins in order to reverse it. </strong></i>
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
 * @date 2023/06/28
 */
public class LinkedListPalindrome {
    public static class ListNode {
        int value;
        ListNode next;

        ListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static boolean isLinkedListPalindrome(ListNode head) {
        // Base cases: an empty list or a single node list is a palindrome
        if (head == null || head.next == null) {
            return true;
        }

        // Find the middle node of the linked list
        ListNode middleNode = findMiddleNode(head);

        // Reverse the second half of the linked list starting from middleNode.next
        ListNode reversedHalf = reverseLinkedList(middleNode.next);

        // Compare the values of nodes in the first half (p1) and reversed second half (p2)
        ListNode p1 = head;
        ListNode p2 = reversedHalf;

        while (p2 != null) {
            if (p1.value != p2.value) {
                // If values are not equal, it's not a palindrome
                return false; 
            }
            p1 = p1.next;
            p2 = p2.next;
        }

        // All values matched, it's a palindrome
        return true; 
    }

    // Helper method to find the middle node of the linked list using the slow-fast pointer technique
    private static ListNode findMiddleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast.next != null && fast.next.next != null) {
            // Slow pointer moves one step
            slow = slow.next; 
            // Fast pointer moves two steps
            fast = fast.next.next; 
        }

        // When fast reaches the end, slow will be at the middle node
        return slow; 
    }

    // Helper method to reverse the linked list starting from the given head
    private static ListNode reverseLinkedList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;

        while (current != null) {
            // Store the next node
            ListNode next = current.next; 
            // Reverse the next pointer
            current.next = prev; 
            // Move prev and current one step forward
            prev = current; 
            current = next;
        }

        // Return the new head of the reversed linked list
        return prev; 
    }

    public static void main(String[] args) {
        // Create the linked list: 0 -> 1 -> 2 -> 2 -> 1 -> 0
        ListNode head = new ListNode(0);
        head.next = new ListNode(1);
        head.next.next = new ListNode(2);
        head.next.next.next = new ListNode(2);
        head.next.next.next.next = new ListNode(1);
        head.next.next.next.next.next = new ListNode(0);

        // Check if the linked list is a palindrome
        boolean isPalindrome = isLinkedListPalindrome(head);
        // Output: true
        System.out.println(isPalindrome); 
    }
}

```

