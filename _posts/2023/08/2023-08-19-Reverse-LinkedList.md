---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Reverse LinkedList"
date: "2023-08-19"
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

5.**The current node points to the next node, `curr = nex`**

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

