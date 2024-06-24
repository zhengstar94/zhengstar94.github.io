---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Sum of Linked Lists"
date: "2023-05-17"
categories:
  - "Algorithm LinkedLists"
---

# Sum of Linked Lists [Medium]

- You're given two Linked Lists of potentially unequal length. Each Linked List represents a non-negative integer, where each node in the Linked List is a digit of that integer, and the first node in each Linked List always represents the least significant digit of the integer. Write a function that returns the head of a new Linked List that represents the sum of the integers represented by the two input Linked Lists.

- Each LinkedList node has an integer value as well as a next node pointing to the next node in the list or to None / null if it's the tail of the list. The value of each LinkedList node is always in the range of 0 - 9.

- Note: your function must create and return a new Linked List, and you're not allowed to modify either of the input Linked Lists.



**Sample Input **

```
linkedListOne = 2 -> 4 -> 7 -> 1
linkedListTwo = 9 -> 4 -> 5
```

**Sample Output **

```
1 -> 9 -> 2 -> 2
// linkedListOne represents 1742
// linkedListTwo represents 549
// 1742 + 549 = 2291
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong>If you can determine the integers that each individual Linked List represents, then all you need to do is add these integers and create a new Linked List that represents the summed value. </strong></i>
</details>





<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>If you go with the approach mentioned in Hint #1, you'll need to break down the sum of the two Linked Lists' numbers into its individual digits. Once you know these digits, you can create a new Linked List using them. This approach is fine, but you can solve this problem more elegantly, with a single iteration through the Linked Lists. </strong></i>
</details>





<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong>Is it necessary to know the entire numbers represented by both Linked Lists in order to calculate their sum? Think back to your elementary-school math class; how did you add two numbers together? </strong></i>
</details>




<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong>Since each Linked List's digits are ordered from least significant digit to most significant digit, you can simply loop through both Linked Lists, consider the digits with the same significance, and add these digits together while keeping track of any carry that comes out of the addition. At each iteration, when you add the two Linked List digits, also add the carry from the previous iteration. Create a new Linked List node that stores the calculated value, and add that to your new Linked List. Keep iterating until you reach the end of both Linked Lists and have no remaining carry. </strong></i>
</details>




<br>



## Method 1

```tex
【O(max(n, m))time∣O(max(n, m))space】
```

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2023/06/14
 */
public class SumOfLinkedLists {
    static class ListNode {
        int val;
        ListNode next;

        public ListNode(int val) {
            this.val = val;
            this.next = null;
        }
    }

    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        int carry = 0;

        while (l1 != null || l2 != null) {
            int sum = carry;

            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            carry = sum / 10;
        }

        if (carry > 0) {
            curr.next = new ListNode(carry);
        }

        return dummy.next;
    }

    public static void main(String[] args) {
        // Build linked list 1
        ListNode node1 = new ListNode(2);
        ListNode node2 = new ListNode(4);
        ListNode node3 = new ListNode(7);
        ListNode node4 = new ListNode(1);
        node1.next = node2;
        node2.next = node3;
        node3.next = node4;

        // Build linked list 2
        ListNode node5 = new ListNode(9);
        ListNode node6 = new ListNode(4);
        ListNode node7 = new ListNode(5);
        node5.next = node6;
        node6.next = node7;

        // Calculate linked lists and

        ListNode result = addTwoNumbers(node1, node5);
        
        while (result != null) {
            System.out.print(result.val + " -> ");
            result = result.next;
        }
        System.out.println("null");
    }

}

```

