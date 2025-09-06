---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "双指针 - 左右指针"
date: "2025-09-06"
pretty_table: true
categories: 
  - "Data Structure"
giscus_comments: true
---

## 介绍

左右指针（对撞）常用于 **排序数组或区间问题**，例如二数之和、三数之和、接雨水等。简单来说，就是用两个指针分别指向区间两端，根据条件向中间收缩或移动，通过利用数组或区间的单调性裁剪搜索空间。这样算法通常也能做到线性复杂度。


## 1. 二数之和（有序数组）

LeetCode 例子：**167. Two Sum II - Input array is sorted**

题意：给定排序数组 `numbers` 和目标值 `target`，返回两个数的下标，使它们之和等于 `target`。

### 思路要点

- 左指针 `left` 指向数组开头，右指针 `right` 指向数组末尾；
- 计算 `sum = numbers[left] + numbers[right]`；
- 若 `sum == target`，返回结果；
- 若 `sum < target`，左指针右移；
- 若 `sum > target`，右指针左移。

### 核心代码

```java
int left = 0, right = numbers.length - 1;
while (left < right) {
    int sum = numbers[left] + numbers[right];
    if (sum == target) return new int[]{left + 1, right + 1};
    else if (sum < target) left++;
    else right--;
}
return new int[]{-1, -1};
```

### 完整函数写法

```java
public int[] twoSum(int[] numbers, int target) {
    int left = 0, right = numbers.length - 1;
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        if (sum == target) return new int[]{left + 1, right + 1};
        else if (sum < target) left++;
        else right--;
    }
    return new int[]{-1, -1};
}
```

---

## 2. 三数之和

LeetCode 例子：**15. 3Sum**

题意：给定数组 `nums`，找出所有不重复的三元组使得和为 0。

### 思路要点

- 先排序数组；
- 固定一个数 `nums[i]`，对剩余子数组使用左右指针寻找两数之和为 `-nums[i]`；
- 遇到重复元素跳过，避免重复三元组。

### 核心代码

```java
Arrays.sort(nums);
List<List<Integer>> res = new ArrayList<>();
for (int i = 0; i < nums.length - 2; i++) {
    if (i > 0 && nums[i] == nums[i - 1]) continue;
    int left = i + 1, right = nums.length - 1;
    while (left < right) {
        int sum = nums[i] + nums[left] + nums[right];
        if (sum == 0) {
            res.add(Arrays.asList(nums[i], nums[left], nums[right]));
            while (left < right && nums[left] == nums[left + 1]) left++;
            while (left < right && nums[right] == nums[right - 1]) right--;
            left++; right--;
        } else if (sum < 0) left++;
        else right--;
    }
}
return res;
```

### 完整函数写法

```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        int left = i + 1, right = nums.length - 1;
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            if (sum == 0) {
                res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;
                left++; right--;
            } else if (sum < 0) left++;
            else right--;
        }
    }
    return res;
}
```

---

## 3. 盛水容器

LeetCode 例子：**11. Container With Most Water**

题意：给定数组 `height`，每个元素代表柱子高度，求能盛水的最大面积。

### 思路要点

- 左右指针指向两端；
- 面积 = min(height[left], height[right]) * (right - left)；
- 移动高度较小的指针，尝试获得更大面积；
- 重复直到左右指针相遇。

### 核心代码

```java
int left = 0, right = height.length - 1, maxArea = 0;
while (left < right) {
    int area = Math.min(height[left], height[right]) * (right - left);
    maxArea = Math.max(maxArea, area);
    if (height[left] < height[right]) left++;
    else right--;
}
return maxArea;
```

### 完整函数写法

```java
public int maxArea(int[] height) {
    int left = 0, right = height.length - 1, maxArea = 0;
    while (left < right) {
        int area = Math.min(height[left], height[right]) * (right - left);
        maxArea = Math.max(maxArea, area);
        if (height[left] < height[right]) left++;
        else right--;
    }
    return maxArea;
}
```

---

## 4. 接雨水（柱状图）

LeetCode 例子：**42. Trapping Rain Water**

题意：给定数组 `height` 表示柱状图高度，计算柱状图能接的雨水总量。

### 思路要点

- 左右指针指向两端，维护 `leftMax` 和 `rightMax` 为左右最高柱高度；
- 若 `height[left] < height[right]`：雨水量由 `leftMax` 决定，更新 `leftMax` 并累加雨水；左指针右移；
- 否则：雨水量由 `rightMax` 决定，更新 `rightMax` 并累加雨水；右指针左移；
- 重复直到左右指针相遇。

### 核心代码

```java
int left = 0, right = height.length - 1;
int leftMax = 0, rightMax = 0, water = 0;
while (left < right) {
    if (height[left] < height[right]) {
        leftMax = Math.max(leftMax, height[left]);
        water += leftMax - height[left];
        left++;
    } else {
        rightMax = Math.max(rightMax, height[right]);
        water += rightMax - height[right];
        right--;
    }
}
return water;
```

### 完整函数写法

```java
public int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int leftMax = 0, rightMax = 0, water = 0;
    while (left < right) {
        if (height[left] < height[right]) {
            leftMax = Math.max(leftMax, height[left]);
            water += leftMax - height[left];
            left++;
        } else {
            rightMax = Math.max(rightMax, height[right]);
            water += rightMax - height[right];
            right--;
        }
    }
    return water;
}
```

---

## 5. 回文 / 对称判断

LeetCode 例子：**234. Palindrome Linked List**

题意：判断链表是否回文。

### 思路要点

- 使用快慢指针找到链表中点；
- 反转前半部分链表（或使用栈存储前半部分节点值）；
- 对比中点后半部分与前半部分元素是否相同；
- 若完全相同则回文，否则不是。

### 核心代码（反转前半部分法）

```java
if (head == null || head.next == null) return true;

// 找中点
ListNode slow = head, fast = head;
while (fast.next != null && fast.next.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}

// 反转前半部分
ListNode prev = null, curr = head;
while (curr != slow.next) {
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}

// 对比前半部分和后半部分
ListNode p1 = prev, p2 = slow.next;
while (p1 != null && p2 != null) {
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

    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    ListNode prev = null, curr = head;
    while (curr != slow.next) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }

    ListNode p1 = prev, p2 = slow.next;
    while (p1 != null && p2 != null) {
        if (p1.val != p2.val) return false;
        p1 = p1.next;
        p2 = p2.next;
    }
    return true;
}
```

## 总结：左右指针

|左右指针类型|核心目标|指针变化|状态维护|统计方式|面试高频题|
|---|---|---|---|---|---|
|**二数之和（有序数组）**|找到和为目标值的两个数|左右指针分别从两端向中间移动|无额外结构，仅根据 sum 判断移动方向|相遇或 sum 满足条件即记录结果|LC 167, Two Sum II|
|**三数之和 / 四数之和**|找到和为目标值的三元组/四元组|固定一个或两个元素，剩余子数组左右指针收缩|跳过重复元素保证结果唯一|每次 sum 满足条件时记录结果|LC 15, 3Sum; LC 18, 4Sum|
|**盛水容器 / 最大面积**|求两端形成的容器最大面积|左右指针向中间移动，每次移动较矮的指针|无需额外结构|每次计算面积并更新最大值|LC 11, Container With Most Water|
|**接雨水（柱状图 / 容量问题）**|求容器能接的水量|左右指针根据高度向中间收缩|维护左右最高高度|每次更新可接水量累加|LC 42, Trapping Rain Water|
|**回文/对称判断**|判断字符串或数组是否回文|左右指针从两端向中间收缩|无需额外结构|相邻元素对比是否一致|常见面试题|
