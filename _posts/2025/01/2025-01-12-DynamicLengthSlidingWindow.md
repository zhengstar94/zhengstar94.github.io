---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "双指针 - 滑动窗口"
date: "2025-09-06"
pretty_table: true
categories: 
  - "Data Structure"
giscus_comments: true
---

## 介绍
滑动窗口用于“连续子串/子数组”的最长/最短/计数类问题。简单来说，滑动窗口就是使用 2 个指针维护一个"活动区间"，右指针扩张窗口，左指针在不满足或满足条件时收缩。这样算法通常能做到线性复杂度。


## 定长窗口（固定长度 k）

LeetCode 例子：**643. Maximum Average Subarray I**

题意就是找长度为 k 的子数组的最大平均值，本质上等价于固定长度子数组的最大和问题。

### 思路要点

固定长度的滑动窗口其实很直接：

1. **右指针往前走**，新元素加进窗口和；
2. 当窗口长度到达 k，就可以结算一次答案；
3. 然后把左端移出（减掉 nums[left]），left++，继续下一个窗口。

这样窗口始终保持长度 = k，更新完全是 O(1) 的。

### 核心代码

```java
int left = 0, windowSum = 0, best = Integer.MIN_VALUE;
for (int right = 0; right < nums.length; right++) {
    windowSum += nums[right];           // 扩张：加上右端
    if (right - left + 1 == k) {        // 窗口长度正好等于 k
        best = Math.max(best, windowSum); 
        windowSum -= nums[left];        // 收缩：移出左端
        left++;
    }
}

```


### 完整函数写法

```java
public int maxSumOfSizeK(int[] nums, int k) {
    int left = 0, windowSum = 0, best = Integer.MIN_VALUE;
    for (int right = 0; right < nums.length; right++) {
        windowSum += nums[right];
        if (right - left + 1 == k) {
            best = Math.max(best, windowSum); // 结算一次答案
            windowSum -= nums[left];          // 左端移出
            left++;
        }
    }
    return best;
}
```


## 变长窗口（求最长）

LeetCode 例子：**3. Longest Substring Without Repeating Characters**

题意：找最长的不含重复字符的子串。

### 思路要点

变长窗口和定长的差别在于：

- **右指针一直扩张**，新元素加入窗口；
- 一旦违反约束（比如出现重复字符），就用 **while** 循环收缩左指针，直到窗口重新合法；
- 每次窗口满足条件，就可以尝试更新答案。

核心技巧就是「**违规就收缩到刚刚合法**」，这样不会错过最优解。

### 核心代码

```java
Map<Character, Integer> window = new HashMap<>();
int left = 0, best = 0;
for (int right = 0; right < s.length(); right++) {
    char c = s.charAt(right);
    window.put(c, window.getOrDefault(c, 0) + 1); // 加入右端

    // 出现重复时收缩左边
    while (window.get(c) > 1) {
        char d = s.charAt(left);
        window.put(d, window.get(d) - 1);
        if (window.get(d) == 0) window.remove(d);
        left++;
    }

    // 到这里窗口已经合法，可以尝试更新答案
    best = Math.max(best, right - left + 1);
}
```


### 完整函数写法

```java
import java.util.*;

public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> window = new HashMap<>();
    int left = 0, best = 0;
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);

        // 收缩直到没有重复
        while (window.get(c) > 1) {
            char d = s.charAt(left);
            window.put(d, window.get(d) - 1);
            if (window.get(d) == 0) window.remove(d);
            left++;
        }

        best = Math.max(best, right - left + 1);
    }
    return best;
}

```


## 变长窗口（求最短覆盖）

LeetCode 例子：**209. Minimum Size Subarray Sum**

题意：给定一个正整数数组 `nums` 和一个目标值 `target`，找出和 ≥ `target` 的最短子数组长度。如果不存在则返回 0。

### 思路要点

- - 右指针不断扩张，把元素加进窗口和；
- 当窗口和 ≥ target 时，开始收缩左指针，尽量缩短窗口；
- 收缩过程中更新「最短长度」。

### 核心代码

```java
int left = 0, windowSum = 0, best = Integer.MAX_VALUE;
for (int right = 0; right < nums.length; right++) {
    windowSum += nums[right];

    // 满足条件，尝试收缩
    while (windowSum >= target) {
        best = Math.min(best, right - left + 1);
        windowSum -= nums[left];
        left++;
    }
}
return best == Integer.MAX_VALUE ? 0 : best;
```


### 完整函数写法

```java
public int minSubArrayLen(int target, int[] nums) {
    int left = 0, windowSum = 0, best = Integer.MAX_VALUE;
    for (int right = 0; right < nums.length; right++) {
        windowSum += nums[right];
        while (windowSum >= target) {
            best = Math.min(best, right - left + 1);
            windowSum -= nums[left];
            left++;
        }
    }
    return best == Integer.MAX_VALUE ? 0 : best;
}

```


## 计数型窗口（统计子数组个数）

LeetCode 例子：**992. Subarrays with K Different Integers**

题意：给定一个整数数组 `nums` 和一个整数 `K`，统计数组中恰好有 K 个不同整数的子数组个数。  核心技巧是先写一个 **atMost(K)** 函数，统计至多 K 个不同整数的子数组个数，再用  `exactly(K) = atMost(K) - atMost(K - 1)`。

### 思路要点

- 右指针扩张，把元素加入窗口，并更新窗口中每个元素的计数；
- 当窗口中的不同元素数 > K 时，收缩左指针，移出不必要的元素；
- 每次循环时，窗口内以 `right` 结尾的所有子数组个数是 `right - left + 1`，累加到结果。

### 核心代码

```java
int left = 0, count = 0;
Map<Integer, Integer> window = new HashMap<>();
for (int right = 0; right < nums.length; right++) {
    int c = nums[right];
    window.put(c, window.getOrDefault(c, 0) + 1);

    // 种类数超过 K，收缩左指针
    while (window.size() > K) {
        int d = nums[left];
        window.put(d, window.get(d) - 1);
        if (window.get(d) == 0) window.remove(d);
        left++;
    }

    count += right - left + 1;
}
return count;
```


### 完整函数写法

```java
import java.util.*;

public int subarraysWithKDistinct(int[] nums, int K) {
    return atMostK(nums, K) - atMostK(nums, K - 1);
}

private int atMostK(int[] nums, int K) {
    int left = 0, count = 0;
    Map<Integer, Integer> window = new HashMap<>();
    for (int right = 0; right < nums.length; right++) {
        int c = nums[right];
        window.put(c, window.getOrDefault(c, 0) + 1);

        while (window.size() > K) {
            int d = nums[left];
            window.put(d, window.get(d) - 1);
            if (window.get(d) == 0) window.remove(d);
            left++;
        }

        count += right - left + 1;
    }
    return count;
}

```


## 单调队列窗口（快速求最大/最小值）

LeetCode 例子：**239. Sliding Window Maximum**

题意：给定一个整数数组 `nums` 和一个窗口大小 `k`，求每个窗口内的最大值。  
核心技巧是用 **双端队列（deque）维护窗口单调递减**，保证队头总是最大值，窗口移动时及时剔除过期或较小的元素。
### 思路要点

- 右指针扩张，将元素加入队列前先剔除队尾小于当前元素的值，保持单调递减；
- 当窗口左端移出时，队头元素如果等于移出的值，需要同步移除；
- 每次窗口长度达到 `k` 时，队头就是该窗口最大值，加入结果。

### 核心代码

```java
Deque<Integer> deque = new ArrayDeque<>();
int[] res = new int[nums.length - k + 1];
int ri = 0;

for (int i = 0; i < nums.length; i++) {
    // 移除队尾比当前元素小的，保持单调递减
    while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
        deque.pollLast();
    }
    deque.offerLast(i);

    // 移除队头过期元素
    if (deque.peekFirst() <= i - k) {
        deque.pollFirst();
    }

    // 当窗口长度达到 k，记录最大值
    if (i >= k - 1) {
        res[ri++] = nums[deque.peekFirst()];
    }
}
return res;

```


### 完整函数写法

```java
import java.util.*;

public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length == 0) return new int[0];

    Deque<Integer> deque = new ArrayDeque<>();
    int[] res = new int[nums.length - k + 1];
    int ri = 0;

    for (int i = 0; i < nums.length; i++) {
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        deque.offerLast(i);

        if (deque.peekFirst() <= i - k) {
            deque.pollFirst();
        }

        if (i >= k - 1) {
            res[ri++] = nums[deque.peekFirst()];
        }
    }
    return res;
}

```

## 总结

|窗口类型|核心目标|窗口变化|窗口维护|统计方式|Leetcode|
|---|---|---|---|---|---|
|**定长窗口**|求固定长度子数组的最大/最小/平均值|右指针推进，长度满 k 时左指针同步右移|进入元素加法/减法，O(1) 更新|满足长度时更新结果|LC 643, 最大平均子数组|
|**变长窗口（求最长）**|求最长满足条件的子串/子数组|右指针扩张，违反条件收缩左指针|Map/Set/计数表维护状态|每次合法窗口更新最长|LC 3, 最长不含重复字符子串|
|**变长窗口（求最短 / 覆盖）**|求最短子串/子数组满足条件|右指针扩张，条件满足收缩左指针|Map/计数/窗口和维护状态|收缩过程中更新最短|LC 209, 76|
|**计数型窗口**|统计满足条件的子数组/子串个数|右指针扩张，条件超限收缩左指针|Map/Set/计数表维护种类或频率|窗口长度累加（right-left+1）|LC 992, 至多/恰好 K 个不同整数|
|**单调队列窗口**|窗口内快速求最大/最小|右指针扩张，队尾剔除不符合单调的元素|双端队列维护单调递减/递增|队头即窗口最大/最小|LC 239, 滑动窗口最大值|
