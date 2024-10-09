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

## Example of Leetode

1. LeetCode 142 - Linked List Cycle II

Problem: Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        val = x;
        next = null;
    }
}

public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) return null;
        
        ListNode slow = head;
        ListNode fast = head;

        // Phase 1: Detect the intersection point
        while (fast != null && fast.next != null) {
            slow = slow.next;            // Move slow pointer one step
            fast = fast.next.next;       // Move fast pointer two steps
            if (slow == fast) break;     // They meet at the cycle
        }

        // If fast reaches null, there is no cycle
        if (fast == null || fast.next == null) return null;

        // Phase 2: Find the entrance to the cycle
        slow = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }

        return slow;  // This is the entrance to the cycle
    }
}

```

2. LeetCode 202 - Happy Number

Problem: Write an algorithm to determine if a number `n` is a "happy number." A happy number is one where you repeatedly replace the number by the sum of the squares of its digits until it either equals `1` (which means it's happy) or loops endlessly in a cycle.

```java
public class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = getNext(n);

        // Phase 1: Detect cycle
        while (fast != 1 && slow != fast) {
            slow = getNext(slow);             // Slow pointer moves one step
            fast = getNext(getNext(fast));    // Fast pointer moves two steps
        }

        // If fast reaches 1, it's a happy number
        return fast == 1;
    }

    private int getNext(int n) {
        int totalSum = 0;
        while (n > 0) {
            int d = n % 10;
            n = n / 10;
            totalSum += d * d;
        }
        return totalSum;
    }
}

```
3. LeetCode 41 - First Missing Positive

Problem: Given an unsorted integer array, find the smallest missing positive integer. Although this problem doesn't directly use Floyd's algorithm, you can solve it by rearranging the array to simulate the cyclic behavior.

```java
public class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;

        // Place each number at its correct index (e.g., 1 goes to index 0, 2 goes to index 1)
        for (int i = 0; i < n; i++) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                // Swap nums[i] with nums[nums[i] - 1]
                swap(nums, i, nums[i] - 1);
            }
        }

        // Find the first missing positive number
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }

        return n + 1;  // If all numbers are in the correct place, return n+1
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}

```

4. Cycle Detection in Function Iteration

In function iteration, we want to detect if a function eventually enters a cycle. You can apply Floyd’s algorithm here as well. Below is an example:

```java
public class Solution {
    public int detectCycleInFunction(int x) {
        int slow = f(x);
        int fast = f(f(x));

        // Phase 1: Detect cycle
        while (slow != fast) {
            slow = f(slow);           // Slow pointer moves one step
            fast = f(f(fast));        // Fast pointer moves two steps
        }

        // Phase 2: Find the entrance of the cycle
        slow = x;
        while (slow != fast) {
            slow = f(slow);
            fast = f(fast);
        }

        return slow;  // Return the entrance of the cycle
    }

    private int f(int x) {
        // Example of a function f
        return (x * x + 1) % 10;  // For instance, square the number and add 1
    }
}

```

5. Cycle Detection in Finite State Machines

You can also use Floyd’s algorithm to detect cycles in state transitions of a finite state machine (FSM).

```java
public class Solution {
    public int detectCycleInFSM(int state, int[][] transitions) {
        int slow = transitions[state][0];
        int fast = transitions[transitions[state][0]][0];

        // Phase 1: Detect cycle
        while (slow != fast) {
            slow = transitions[slow][0];               // Slow pointer moves one step
            fast = transitions[transitions[fast][0]][0];  // Fast pointer moves two steps
        }

        // Phase 2: Find the entrance of the cycle
        slow = state;
        while (slow != fast) {
            slow = transitions[slow][0];
            fast = transitions[fast][0];
        }

        return slow;  // Return the state where the cycle starts
    }
}

```