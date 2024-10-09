---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Floyd's Algorithm for Finding Duplicate Number"
date: "2024-10-09"
categories: 
  - "Data Structure"
---


## 1. Introduction

Floyd's Cycle-Finding Algorithm, also known as the "Tortoise and Hare" algorithm, is a pointer algorithm that uses only two pointers, moving through the sequence at different speeds. This algorithm is particularly useful for detecting cycles in sequences and has an interesting application in solving the "Find the Duplicate Number" problem.

## 2. Problem Statement

Given an array `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive, prove that at least one duplicate number must exist. Assume that there is only one duplicate number, but it may be repeated more than once.

The task is to find the duplicate number without modifying the array `nums` and using only constant extra space.

## 3. Algorithm Description

Floyd's algorithm can be adapted to solve this problem by treating the array as a linked list where `nums[i]` is treated as a pointer to index `nums[i]`.

The algorithm consists of two phases:

### Phase 1: Detecting the intersection point of two runners

1. Initialize two pointers, `tortoise` and `hare`, to the first element of the array.
2. Move `tortoise` one step at a time: `tortoise = nums[tortoise]`
3. Move `hare` two steps at a time: `hare = nums[nums[hare]]`
4. Repeat steps 2 and 3 until `tortoise` and `hare` meet at the same element.

### Phase 2: Finding the entrance to the cycle (the duplicate number)

1. Reset the `tortoise` to the first element of the array.
2. Move both `tortoise` and `hare` one step at a time.
3. The point at which they meet is the entrance to the cycle, which is the duplicate number.

## 4. Implementation

```java
public class Solution {
  public int findDuplicate(int[] nums) {
    // Phase 1: Detecting the cycle
    int tortoise = nums[0];  // Initialize the slow pointer (tortoise)
    int hare = nums[0];      // Initialize the fast pointer (hare)

    do {
      // Move the tortoise one step forward
      tortoise = nums[tortoise];

      // Move the hare two steps forward
      hare = nums[nums[hare]];

      // Continue until the tortoise and hare meet
      // This meeting point is guaranteed to be inside the cycle
    } while (tortoise != hare);

    // Phase 2: Finding the entrance to the cycle (the duplicate number)
    tortoise = nums[0];  // Reset the tortoise to the start of the array

    // Move both pointers at the same speed until they meet again
    while (tortoise != hare) {
      // Move both pointers one step at a time
      tortoise = nums[tortoise];
      hare = nums[hare];

      // The point where they meet is the entrance to the cycle,
      // which is the duplicate number
    }

    // Both pointers now point to the duplicate number
    return hare;  // We could return tortoise as well, they're the same at this point
  }
}
```

## 5. Proof of Correctness

The correctness of this algorithm relies on the following properties:

1. If there is a cycle, the `tortoise` and `hare` will eventually meet.
2. The meeting point is not necessarily the duplicate number, but it is guaranteed to be part of the cycle.
3. The distance from the start of the array to the entrance of the cycle (duplicate number) is equal to the distance from the meeting point to the entrance of the cycle.

## 6. Complexity Analysis

- Time Complexity: O(n), where n is the length of the array. The worst case occurs when the duplicate element is at the end of the array.
- Space Complexity: O(1), as only two pointers are used regardless of the input size.

## 7. Advantages and Disadvantages

### Advantages:
- Meets the problem constraints of O(1) space complexity.
- Does not modify the original array.
- Has optimal time complexity of O(n).

### Disadvantages:
- Not intuitive and can be difficult to understand at first glance.
- Doesn't provide information about all duplicates if multiple exist.

## 8. Conclusion

Floyd's Cycle-Finding Algorithm provides an elegant and efficient solution to the "Find the Duplicate Number" problem. While it may not be the most intuitive approach, it showcases how algorithmic thinking can lead to optimal solutions that satisfy strict constraints. Understanding this algorithm and its application can provide valuable insights into problem-solving techniques in computer science.