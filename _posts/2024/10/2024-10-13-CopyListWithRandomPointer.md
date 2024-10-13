---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "138.Copy List with Random Pointer"
date: "2024-10-13"
categories:
  - "LeetCode LinkedList"
---


- A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.
- Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.
- For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.
- Return *the head of the copied linked list*.
- The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:
  - `val`: an integer representing `Node.val`
  - `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.
- Your code will **only** be given the `head` of the original linked list.


**Example 1**

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**Example 2**

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```

**Example 3**

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.LinkList;

/**
 * @author zhengstars
 * @date 2024/10/13
 */
public class CopyListWithRandomPointer {
    public static Node copyRandomList(Node head) {
        if (head == null) {
            return null; 
        }

        // Step 1: Create copy nodes and insert them after original nodes
        Node curr = head;
        while (curr != null) {
            // Create a new node with the same value as the current node
            Node copy = new Node(curr.val);
            // Insert the new node right after the current node
            copy.next = curr.next;
            curr.next = copy;
            // Move to the next original node
            curr = copy.next;
        }

        // Step 2: Set random pointers for the copy nodes
        curr = head;
        while (curr != null) {
            // If the original node has a random pointer, set the copy's random pointer
            if (curr.random != null) {
                // curr.next is the copy of curr
                // curr.random.next is the copy of curr.random
                curr.next.random = curr.random.next;
            }
            // Move to the next original node
            curr = curr.next.next;
        }

        // Step 3: Separate the original and copied lists
        Node dummy = new Node(0); // Dummy node to start the copied list
        Node copyCurr = dummy;
        curr = head;
        while (curr != null) {
            // Connect the copy nodes
            copyCurr.next = curr.next;
            // Restore the original list connection
            curr.next = curr.next.next;
            // Move both pointers
            copyCurr = copyCurr.next;
            curr = curr.next;
        }

        // Return the head of the copied list
        return dummy.next;
    }

    // Test cases
    public static void main(String[] args) {

        // Test case 1: [ [ 7,null],[13,0],[11,4],[10,2],[1,0] ]
        Node head1 = new Node(7);
        Node node13 = new Node(13);
        Node node11 = new Node(11);
        Node node10 = new Node(10);
        Node node1 = new Node(1);
        head1.next = node13;
        node13.next = node11;
        node11.next = node10;
        node10.next = node1;
        head1.random = null;
        node13.random = head1;
        node11.random = node1;
        node10.random = node11;
        node1.random = head1;

        Node result1 = copyRandomList(head1);
        System.out.println("Test case 1 passed: " + validateCopy(head1, result1));

        // Test case 2: [ [ 1,1],[2,1] ]
        Node head2 = new Node(1);
        Node node2 = new Node(2);
        head2.next = node2;
        head2.random = node2;
        node2.random = node2;

        Node result2 = copyRandomList(head2);
        System.out.println("Test case 2 passed: " + validateCopy(head2, result2));

        // Test case 3: [ [ 3,null],[3,0],[3,null] ]
        Node head3 = new Node(3);
        Node node3_2 = new Node(3);
        Node node3_3 = new Node(3);
        head3.next = node3_2;
        node3_2.next = node3_3;
        head3.random = null;
        node3_2.random = head3;
        node3_3.random = null;

        Node result3 = copyRandomList(head3);
        System.out.println("Test case 3 passed: " + validateCopy(head3, result3));
    }

    private static boolean validateCopy(Node original, Node copy) {
        while (original != null && copy != null) {
            if (original.val != copy.val) {
                return false;
            }
            if ((original.random == null && copy.random != null) ||
                    (original.random != null && copy.random == null)) {
                return false;
            }
            if (original.random != null && original.random.val != copy.random.val) {
                return false;
            }
            original = original.next;
            copy = copy.next;
        }
        return original == null && copy == null;
    }
}

class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
```





