---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Sort Stack"
date: "2023-05-04"
categories:
  - "Algorithm Stack"
---

# Sort Stack [Medium]

- Write a function that takes in an array of integers representing a stack, recursively sorts the stack in place (i.e., doesn't create a brand new array), and returns it.
- The array must be treated as a stack, with the end of the array as the top of the stack. Therefore, you're only allowed to
  - Pop elements from the top of the stack by removing elements from the end of the array using the built-in .pop() method in your programming language of choice.
  - Push elements to the top of the stack by appending elements to the end of the array using the built-in .append() method in your programming language of choice.
  - Peek at the element on top of the stack by accessing the last element in the array.
- You're not allowed to perform any other operations on the input array, including accessing elements (except for the last element), moving elements, etc.. You're also not allowed to use any other data structures, and your solution must be recursive.

**Sample Input **

```
stack = [-5, 2, -2, 4, 3, 1]
```

**Sample Output **

```
[-5, -2, 1, 2, 3, 4]
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> If you had to insert a single item into an already sorted stack, all the while abiding by the constraints of this problem, how would you do that? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Inserting a single item in an already sorted stack is fairly simple: you can pop elements off of the stack until you find an element that's smaller than or equal to the value that you want to add. Then, you can push that value on top of the stack and reinsert all the previously popped items back on top of the stack in the reverse order in which you popped them off. The resulting stack will still be sorted.</strong></i>
</details>

<br>



<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> You can easily insert multiple items in an already sorted stack by just repeatedly performing what's described in Hint #2. However, you'll need to have an already sorted stack. To get an already sorted stack, you'll need to pop all of the elements off the unsorted stack until it's eventually empty, and then you'll need to push all of the items back on the stack, inserting them in their sorted order one at a time.</strong></i>
</details>

<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> If you're thinking about Hint #3 recursively, the steps are the following:<br>
1. Pop an item from the top of the stack, and hold onto it in memory.<br>
2. Sort the rest of the stack. To do so, repeat step #1 until the stack is empty, at which point you've reached the base case since an empty stack is always sorted.<br>
3. Insert the most recently popped off item from step #1 back into the now sorted stack but in its proper sorted position. The first time that you reinsert an item, it'll be inserted in an empty stack.<br> </strong></i>
</details>

<br>

## Method 1

```tex
【O(n^2)time∣O(n)space】
```

```java
package Stack;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/06/04
 */
public class SortStack {
    public static void sortStack(Stack<Integer> stack) {
        if (stack.isEmpty()) {
            return;
        }

        // Retrieve the top element of the stack
        int top = stack.pop();

        // Recursively sort the remaining elements
        sortStack(stack);

        // Insert the top element into the sorted part
        insertSorted(stack, top);
    }

    private static void insertSorted(Stack<Integer> stack, int value) {
        if (stack.isEmpty() || value > stack.peek()) {
            // If the stack is empty or the current value is greater than the top element, insert directly
            stack.push(value);
        } else {
            // Retrieve the top element
            int top = stack.pop();
            // Continue inserting downwards
            insertSorted(stack, value);
            // Push the previously retrieved top element back into the stack
            stack.push(top);
        }
    }
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(-5);
        stack.push(2);
        stack.push(-2);
        stack.push(4);
        stack.push(3);
        stack.push(1);

        sortStack(stack);

        System.out.println(stack);
    }
}

```

