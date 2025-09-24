---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "链表 - 反转 / 合并 / 快慢指针 / 找交点"
date: "2025-09-24"
pretty_table: true
categories: 
  - "Data Structure"
giscus_comments: true
---


## 介绍


- **反转链表**：包括整表反转、区间反转、K 个一组反转；
- **合并链表**：包括合并两个有序链表、合并 K 个有序链表；
- **快慢指针**：用于判环、找环入口、找中点、倒数第 N 个节点、回文判断；
- **找交点**：判断两链表是否相交，找到第一个相交节点。



---

## 1. 反转链表

[反转链表](https://zhengxingxing.com/blog/2025/Reverse-LinkedList/)是链表题基础，必须掌握迭代和递归两种方法，并能扩展到区间反转和 k 组反转。

### 思路要点
- 迭代法：用 `prev`、`curr` 逐节点反转 `next` 指针；
- 递归法：利用递归回溯，将当前节点挂到后面节点的 `next`。

### 核心代码（整表反转 - 迭代）

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### 核心代码（整表反转 - 递归）

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode newHead = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}
```

---

## 2. 合并链表

常见场景：合并两个有序链表或 K 个有序链表。可用迭代、递归或优先队列。

### 思路要点

- 创建 Dummy 节点简化边界；

- 比较两链表当前节点值，选择较小的挂到新链表末尾；

- 移动指针继续比较直到一条链表为空。


### 核心代码（合并两个有序链表）

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0), tail = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            tail.next = l1;
            l1 = l1.next;
        } else {
            tail.next = l2;
            l2 = l2.next;
        }
        tail = tail.next;
    }
    tail.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

---

## 3. 快慢指针

快慢指针通过不同速度指针的相遇、距离差解决链表结构问题。

### 题型与模板

- **判环**（LC 141）：快指针走两步，慢指针走一步，相遇说明有环。

- **找环入口**（LC 142）：相遇后一个指针回头，两指针同步走，相遇点即环入口。

- **找中点**（LC 876）：快走两步慢走一步，快到尾慢在中点。

- **倒数第 N 个节点**（LC 19）：快指针先走 N 步，然后快慢同步走直到快到尾。


### 核心代码（判环）

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

---

## 4. 找交点

判断两条链表是否相交，并找到第一个相交节点。

### 思路要点

- 用两个指针 pA, pB 分别遍历两链表；

- 到尾后切换到另一条链表的头；

- 如果有交点，最终会在交点相遇；若无交点，都会到 null。


### 核心代码

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;
    ListNode pA = headA, pB = headB;
    while (pA != pB) {
        pA = (pA == null) ? headB : pA.next;
        pB = (pB == null) ? headA : pB.next;
    }
    return pA;
}
```

---

## 总结表格

|类别|核心目标|核心技巧|高频题|
|---|---|---|---|
|**反转链表**|改变链表方向|迭代双指针 / 递归回溯|LC 206, 92, 25|
|**合并链表**|合并成有序链表|Dummy 节点 + 双指针|LC 21, 23|
|**快慢指针**|判环、找中点、找倒数第 N 个节点|快走两步慢走一步；或先走 N 步|LC 141, 142, 876, 19, 234|
|**找交点**|判断两链表是否相交|双指针遍历两条链表长度|LC 160|
