---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "双指针 - 快慢指针"
date: "2025-09-06"
pretty_table: true
categories: 
  - "Data Structure"
giscus_comments: true
---

## 介绍

快慢指针用于链表或数组中通过“速度差”获取结构信息。简单来说，就是用两个指针同时遍历，慢指针走得慢、快指针走得快，通过二者的相遇、交汇或间隔距离来判断环、找中点、倒数第 N 个节点或检测回文等。这样算法通常也能做到线性复杂度。


## 1. 判环（环的存在性）

LeetCode 例子：**141. Linked List Cycle**

题意：判断链表中是否存在环。

### 思路要点

- 定义 **快慢指针**：慢指针一次走 1 步，快指针一次走 2 步；
- 若链表存在环，快慢指针最终会在环内相遇；
- 若链表无环，快指针先到达末尾。

### 核心代码

```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) return true; // 相遇则有环
}
return false; // 无环
```

### 完整函数写法

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

## 2. 找环入口

LeetCode 例子：**142. Linked List Cycle II**

题意：返回链表环的起点，如果无环返回 null。

### 思路要点

- 首先使用快慢指针判断环是否存在；
- 相遇后，将一个指针重新指向链表头，两指针同步移动，每次走 1 步；
- 再次相遇点即为环入口。

### 核心代码

```java
ListNode slow = head, fast = head;
// 判断是否有环
do {
    if (fast == null || fast.next == null) return null;
    slow = slow.next;
    fast = fast.next.next;
} while (slow != fast);

// 找环入口
slow = head;
while (slow != fast) {
    slow = slow.next;
    fast = fast.next;
}
return slow;
```

### 完整函数写法

```java
public ListNode detectCycle(ListNode head) {
    ListNode slow = head, fast = head;
    // 判环
    do {
        if (fast == null || fast.next == null) return null;
        slow = slow.next;
        fast = fast.next.next;
    } while (slow != fast);

    // 找环入口
    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow;
}
```

---

## 3. 链表中点查找

LeetCode 例子：**876. Middle of the Linked List**

题意：找到链表的中点节点（偶数长度返回中间偏右节点）。

### 思路要点

- 快指针每次走 2 步，慢指针每次走 1 步；
- 快指针到达末尾时，慢指针恰好在中点位置。

### 核心代码

```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
return slow;
```

### 完整函数写法

```java
public ListNode middleNode(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

---

## 4. 倒数第 N 个节点

LeetCode 例子：**19. Remove Nth Node From End of List**

题意：返回链表倒数第 N 个节点（或删除该节点）。

### 思路要点

- 快指针先走 N 步，使快慢指针间隔 N 个节点；
- 然后快慢指针同时走，直到快指针到达末尾；
- 慢指针指向的就是倒数第 N 个节点前一个节点（方便删除操作）。

### 核心代码（删除倒数第 N 个节点）

```java
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode slow = dummy, fast = dummy;

// 快指针先走 N 步
for (int i = 0; i < n; i++) fast = fast.next;

// 同时移动
while (fast.next != null) {
    slow = slow.next;
    fast = fast.next;
}

// 删除节点
slow.next = slow.next.next;
return dummy.next;
```

### 完整函数写法

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode slow = dummy, fast = dummy;
    for (int i = 0; i < n; i++) fast = fast.next;
    while (fast.next != null) {
        slow = slow.next;
        fast = fast.next;
    }
    slow.next = slow.next.next;
    return dummy.next;
}
```

## 5. 回文检查（数组/链表）

LeetCode 例子：**234. Palindrome Linked List**

题意：判断链表是否是回文结构。

### 思路要点

- 用快慢指针找到链表中点；
- 反转中点后的链表；
- 两段链表逐节点比较，判断是否回文；
- 可选：恢复链表结构。

### 核心代码

```java
// 找中点
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}

// 反转后半段
ListNode prev = null, curr = slow;
while (curr != null) {
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}

// 比较
ListNode p1 = head, p2 = prev;
while (p2 != null) {
    if (p1.val != p2.val) return false;
    p1 = p1.next;
    p2 = p2.next;
}
return true;
```

### 完整函数写法

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) return true;

    // 找中点
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // 反转后半段
    ListNode prev = null, curr = slow;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }

    // 比较两段链表
    ListNode p1 = head, p2 = prev;
    while (p2 != null) {
        if (p1.val != p2.val) return false;
        p1 = p1.next;
        p2 = p2.next;
    }
    return true;
}

```

## 总结：快慢指针

|快慢指针类型|核心目标|指针变化|状态维护|统计方式|面试高频题|
|---|---|---|---|---|---|
|**判环（Cycle Detection）**|判断链表是否有环|慢指针每次走 1 步，快指针每次走 2 步，相遇则有环|无需额外结构，仅指针移动|相遇即判定有环|LC 141, Linked List Cycle|
|**找环入口（Cycle Entry）**|找出链表环的起始节点|快慢指针相遇后，一个从头开始，一个从相遇点，步数同步推进，直到相遇|指针移动和相遇条件|相遇节点即环入口|LC 142, Linked List Cycle II|
|**链表中点**|找到链表中点（偶数取上中/下中根据需求）|慢指针 1 步，快指针 2 步|无需额外维护|慢指针到达即中点|LC 876, Middle of Linked List|
|**倒数第 N 个节点**|找到链表倒数第 N 个节点|快指针先走 N 步，慢指针随后同步推进|记录快慢指针位置|快指针到末尾时慢指针即目标|LC 19, Remove Nth Node From End of List|
|**回文检查**|判断链表或数组是否回文|快慢指针找中点，慢指针反转前半部分或记录栈|链表反转或栈存储前半部分|中点后对比两段元素|LC 234, Palindrome Linked List|
