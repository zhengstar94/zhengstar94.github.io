---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Reverse LinkedList"
date: "2025-09-21"
categories: 
  - "Data Structure"
---

# Reverse Linked List

## Solution (Traverse the linked list, pointing the next of each node to the previous node)

1. Save the next of the current node, `next = curr.next`
2. Point the next of the current node to the previous node, `curr.next = prev`
3. Move the forward pointer back to `prev = curr`
4. Move the current pointer back to `curr = next`


## Step

1.**Initial list and pre node**

   {% include figure.liquid loading="eager" path="assets/img/2023/08/1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2.**Initial list and next node, `next = curr.next`**

   {% include figure.liquid loading="eager" path="assets/img/2023/08/2.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.**The current.next points to the pre node, `curr.next = prev`**

   {% include figure.liquid loading="eager" path="assets/img/2023/08/3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

4.**The pre node points to the current node, `prev = curr`**

   {% include figure.liquid loading="eager" path="assets/img/2023/08/4.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

5.**The current node points to the next node, `curr = next`**

   {% include figure.liquid loading="eager" path="assets/img/2023/08/5.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

6.**The next node moves to the current.next, `next = curr.next`**

   {% include figure.liquid loading="eager" path="assets/img/2023/08/6.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

7.**Loop traversal**

   {% include figure.liquid loading="eager" path="assets/img/2023/08/7.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/9.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/10.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/11.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/12.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/13.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/14.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/15.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/16.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/17.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/18.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/19.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/20.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
   {% include figure.liquid loading="eager" path="assets/img/2023/08/21.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## Code

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2023/08/19
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
        ListNode prev = null;

        ListNode curr = head;

        ListNode next = null;

        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;

        }
        return prev;
    }
}
```

# Reverse Linked List II

## Solution (Iterate and insert the following node of curr to the position after pre)

1. Save the next node of the current node, `ListNode next = curr.next`
2. Let the next pointer of the current node point to the node pointed by next's next pointer. `curr.next = next.next;`
3. Point the next of the `next` node to the node pointed by pre's next pointer . `next.next = pre.next`
4. Move the `next` pointer to the position after the pre pointer, by setting the `next` pointer of the pre poniter to the `next` pointer. `pre.next = next;`


## Step

1.**Initial List and Pre(head) Node**

**left = 2, right = 4. After the pre loop, pre points to 1 and curr points to 2.**

{% include figure.liquid loading="eager" path="assets/img/2025/09/1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2.**Initial List and Next Node, `next = curr.next`**

**Before: curr.next = 3**

{% include figure.liquid loading="eager" path="assets/img/2025/09/2.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.**The current.next points to the pre.next node, `curr.next = next.next`**

**After: curr.next = 4**

{% include figure.liquid loading="eager" path="assets/img/2025/09/3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

4.**The next.next node points to the pre.next. `next.next = pre.next`**

**next.next = pre.next;**
**curr.next = 4**
**next.next = 2**
**pre.next = 2**

{% include figure.liquid loading="eager" path="assets/img/2025/09/4.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

5.**The pre node points to the next node, `pre.next = next`**

**curr.next = 4**
**next.next = 2**
**pre.next = 3**

{% include figure.liquid loading="eager" path="assets/img/2025/09/5.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

6.Loop1

{% include figure.liquid loading="eager" path="assets/img/2025/09/6.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

7.**Repeat step 2 to 5 , loop traversal, until to meet the end of range**

**ListNode next = 4**
**curr.next = 4**
**next.next = 5**
**pre.next = 3**

{% include figure.liquid loading="eager" path="assets/img/2025/09/7.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

8.**Loop traversal**

{% include figure.liquid loading="eager" path="assets/img/2025/09/8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2025/09/9.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2025/09/10.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2025/09/11.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## Code

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2025/09/21
 */
public class ReverseLinkedListII {

    public static class ListNode {
        int value;
        ListNode next;

        public ListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null || left == right) {
            return head;
        }

        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;

        for (int i = 1; i < left; i++) {
            pre = pre.next;
        }

        ListNode curr = pre.next;

        for (int i = 0; i < right - left; i++) {
            ListNode next = curr.next;
            curr.next = next.next;
            next.next = pre.next;
            pre.next = next;
        }

        return dummy.next;
    }
}

```
