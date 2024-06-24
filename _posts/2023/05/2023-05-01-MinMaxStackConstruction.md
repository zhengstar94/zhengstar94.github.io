---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Min Max Stack Construction"
date: "2023-05-01"
categories:
  - "Algorithm Stack"
---

# Min Max Stack Construction [Medium]

- Write a MinMaxStack class for a Min Max Stack. The class should support:
- Pushing and popping values on and off the stack. Peeking at the value at the top of the stack. Getting both the minimum and the maximum values in the stack at any given point in time. All class methods, when considered independently, should run in constant time and with constant space.

**Sample Usage **

```
// All operations below are performed sequentially.
MinMaxStack(): - // instantiate a MinMaxStack
push(5): -
getMin(): 5
getMax(): 5
peek(): 5
push(7): -
getMin(): 5
getMax(): 7
peek(): 7
push(2): -
getMin(): 2
getMax(): 7
peek(): 2
pop(): 2
pop(): 7
getMin(): 5
getMax(): 5
peek(): 5
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> You should be able to push values on, pop values off, and peek at values on top of the stack at any time and in constant time, using constant space. What data structure maintains order and would allow you to do this? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> You should be able to get the minimum and maximum values in the stack at any time and in constant time, using constant space. What data structure would allow you to do this?</strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Since the minimum and maximum values in the stack can change with every push and pop, you will likely need to keep track of all the mins and maxes at every value in the stack. </strong></i>
</details>



<br>

## Method 1

```tex
【O(1)time∣O(1)space】
```

```java
package Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @author zhengstars
 * @date 2023/06/04
 */
public class MinMaxStack {

    /**
     * Store stack elements
     */
    private Deque<Integer> stack;
    /**
     * Current minimum value
     */
    private int min;
    /**
     * Current maximum value
     */
    private int max;

    public MinMaxStack() {
        stack = new ArrayDeque<>();
        min = Integer.MAX_VALUE;
        max = Integer.MIN_VALUE;
    }

    public void push(int value) {
        if (value < min) {
            // Save the previous minimum value before pushing
            stack.push(min);
            // Update the minimum value
            min = value;
        }
        if (value > max) {
            // Save the previous maximum value before pushing
            stack.push(max);
            // Update the maximum value
            max = value;
        }
        // Push the element onto the stack
        stack.push(value);
    }

    public int pop() {
        // Pop the top element from the stack
        int value = stack.pop();
        if (value == max) {
            // If the popped element is the maximum value, update the maximum value to the saved value
            max = stack.pop();
        }
        if (value == min) {
            // If the popped element is the minimum value, update the minimum value to the saved value
            min = stack.pop();
        }
        return value;
    }

    public int peek() {
        // Return the top element of the stack
        return stack.peek();
    }

    public int getMin() {
        // Return the minimum value
        return min;
    }

    public int getMax() {
        // Return the maximum value
        return max;
    }

    public static void main(String[] args) {
        MinMaxStack stack = new MinMaxStack();
        stack.push(5);

        // Return 5
        System.out.println(stack.getMin());
        // Return 5
        System.out.println(stack.getMax());
        stack.peek();
        stack.push(7);

        // Return 5
        System.out.println(stack.getMin());
        // Return 7
        System.out.println(stack.getMax());
        stack.peek();
        stack.push(2);

        // Return 2
        System.out.println(stack.getMin());
        // Return 7
        System.out.println(stack.getMax());
        stack.peek();    // Return 2
        stack.pop();     // Pop 2
        stack.pop();     // Pop 7

        // Return 5
        System.out.println(stack.getMin());
        // Return 5
        System.out.println(stack.getMax());
        stack.peek();    // Return 5
    }
}

```

