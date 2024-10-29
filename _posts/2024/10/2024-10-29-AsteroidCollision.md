---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "735.Asteroid Collision"
date: "2024-10-29"
categories:
  - "LeetCode Stack"
---

- We are given an array `asteroids` of integers representing asteroids in a row.
- For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.
- Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example 1**

```
Input: asteroids = [5,10,-5]
Output: [5,10]
Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.
```

**Example 2**

```
Input: asteroids = [8,-8]
Output: []
Explanation: The 8 and -8 collide exploding each other.
```

**Example 3**

```
Input: asteroids = [10,2,-5]
Output: [10]
Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;


import java.util.ArrayDeque;
import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/10/29
 */
public class AsteroidCollision {
    public static int[] asteroidCollision(int[] asteroids) {
        // Use a deque to store asteroids, more efficient than a stack
        ArrayDeque<Integer> q = new ArrayDeque<>();

        for (int asteroid : asteroids) {
            if (q.isEmpty() || asteroid > 0) {
                // Case 1: Queue is empty, add the asteroid directly
                // Case 2: Current asteroid is moving right, no collision with previous asteroids
                q.addLast(asteroid);
            } else {
                // Current asteroid is moving left (asteroid < 0), so handle collisions

                // Loop to handle collisions: if the asteroid at the end of the queue
                // is moving right and has less mass than the current asteroid
                // (q.peekLast() + asteroid < 0 means the right-moving asteroid is smaller in mass
                // because asteroid is negative, and their sum < 0 implies |asteroid| is larger)
                while (!q.isEmpty() && q.peekLast() > 0 && q.peekLast() + asteroid < 0) {
                    q.pollLast(); // Remove the asteroid at the end of the queue (it explodes)
                }

                // Three situations after handling collisions:
                if (q.isEmpty() || q.peekLast() < 0) {
                    // Case 1: Queue is empty, add the asteroid directly
                    // Case 2: Asteroid at the end of the queue is moving left, no collision, add directly
                    q.addLast(asteroid);
                } else if (q.peekLast() + asteroid == 0) {
                    // Case 3: Equal mass, both explode, remove the last asteroid from the queue and do not add the current one
                    q.pollLast();
                }
                // Implicit Case 4: Current asteroid has smaller mass and is destroyed, no action needed
            }
        }

        // Convert the deque to an array
        int[] ans = new int[q.size()];
        for (int i = q.size() - 1; i >= 0; i--) {
            ans[i] = q.pollLast();
        }
        return ans;
    }

    public static void main(String[] args) {
        // Test cases
        int[][] testCases = {
                { 5, 10, -5 },          // No collision, output [ 5, 10 ]
                { 8, -8 },              // Equal mass, both explode, output []
                { 10, 2, -5 },          // -5 collides with 2, 2 explodes, final output [ 10]
                { -2, -1, 1, 2 },       // All asteroids move left or right, no collision, output [ -2, -1, 1, 2 ]
                { 1, -1, 1, -1 },       // Each pair 1 and -1 collide, all explode, output []
                { 10, -10, 5, -15, 20 } // 10 and -10 collide, 5 and -15 collide, output [ 20 ]
        };

        for (int i = 0; i < testCases.length; i++) {
            int[] result = asteroidCollision(testCases[i]);
            System.out.println("Test case " + (i + 1) + ": " + Arrays.toString(result));
        }
    }
}

```





