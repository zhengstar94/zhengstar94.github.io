---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Balanced Brackets"
date: "2023-05-02"
categories:
  - "Algorithm Stack"
---

# Balanced Brackets [Medium]

- Write a function that takes in a string made up of brackets ((, [, {, ), ], and }) and other optional characters. The function should return a boolean representing whether the string is balanced with regards to brackets.
- A string is said to be balanced if it has as many opening brackets of a certain type as it has closing brackets of that type and if no bracket is unmatched. Note that an opening bracket can't match a corresponding closing bracket that comes before it, and similarly, a closing bracket can't match a corresponding opening bracket that comes after it. Also, brackets can't overlap each other as in [(]).

**Sample Input **

```
string = "([])(){}(())()()"
```

**Sample Output **

```
true // it's balanced
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> If you iterate through the input string one character at a time, there are two scenarios in which the string will be unbalanced: either you run into a closing bracket with no prior matching opening bracket or you get to the end of the string with some opening brackets that haven't been matched. Can you use an auxiliary data structure to keep track of all the brackets and efficiently check if you run into a unbalanced scenario at every iteration? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Consider using a stack to store opening brackets as you traverse the string. The Last-In-First-Out property of the stack should allow you to match any closing brackets that you run into against the most recent opening bracket, if one exists, in which case you can simply pop it out of the stack. How can you check that there are no unmatched opening bracket once you've finished traversing through the string?</strong></i>
</details>





<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Stack;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/06/04
 */
public class BalancedBrackets {
    public static boolean isBalanced(String str) {
        Stack<Character> stack = new Stack<>();

        for (char c : str.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') {
                // If it's an opening bracket, push it onto the stack
                stack.push(c);  
            } else if (c == ')' || c == ']' || c == '}') {
                if (stack.isEmpty()) {
                   // If the stack is empty, there is no corresponding opening bracket, return false
                    return false;
                }

                // Pop the top element from the stack
                char top = stack.pop();  
                if ((c == ')' && top != '(') || (c == ']' && top != '[') || (c == '}' && top != '{')) {
                    // If the current closing bracket doesn't match the popped opening bracket, return false
                    return false;
                }
            }
        }

        // Finally, check if the stack is empty. If it is, all brackets are correctly matched, return true; otherwise, return false
        return stack.isEmpty();
    }

    public static void main(String[] args) {
        String str = "([])(){}(())()()";
        boolean balanced = isBalanced(str);
        System.out.println(balanced); // Output: true
    }
}

```

