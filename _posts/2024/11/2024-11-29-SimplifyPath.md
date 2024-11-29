---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "71. Simplify Path"
date: "2024-11-29"
categories:
  - "LeetCode Stack"
---


- You are given an *absolute* path for a Unix-style file system, which always begins with a slash `'/'`. Your task is to transform this absolute path into its **simplified canonical path**.
- The *rules* of a Unix-style file system are as follows:
  - A single period `'.'` represents the current directory.
  - A double period `'..'` represents the previous/parent directory.
  - Multiple consecutive slashes such as `'//'` and `'///'` are treated as a single slash `'/'`.
  - Any sequence of periods that does **not match** the rules above should be treated as a **valid directory or** **file** **name**. For example, `'...' `and `'....'` are valid directory or file names.
- The simplified canonical path should follow these *rules*:
  - The path must start with a single slash `'/'`.
  - Directories within the path must be separated by exactly one slash `'/'`.
  - The path must not end with a slash `'/'`, unless it is the root directory.
  - The path must not have any single or double periods (`'.'` and `'..'`) used to denote current or parent directories.
- Return the **simplified canonical path**.

**Example 1**

```
Input: path = "/home/"
Output: "/home"
Explanation:
The trailing slash should be removed.
```

**Example 2**

```
Input: path = "/home//foo/"
Output: "/home/foo"
Explanation:
Multiple consecutive slashes are replaced by a single one.
```

**Example 3**

```
Input: path = "/home/user/Documents/../Pictures"
Output: "/home/user/Pictures"
Explanation:
A double period ".." refers to the directory up a level (the parent directory).
```

**Example 4**

```
Input: path = "/../"
Output: "/"
Explanation:
Going one level up from the root directory is not possible.
```

**Example 5**

```
Input: path = "/.../a/../b/c/../d/./"
Output: "/.../b/d"
Explanation:
"..." is a valid name for a directory in this problem.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @author zhengxingxing
 * @date 2024/11/29
 */
public class SimplifyPath {
    public static String simplifyPath(String path) {
        // Initialize a double-ended queue (stack) to store valid directory components
        // Using ArrayDeque provides more efficient stack operations compared to traditional Stack
        Deque<String> stack = new ArrayDeque<>();

        // Split the path into components using "/" as the delimiter
        // This handles multiple scenarios like consecutive slashes, leading/trailing slashes
        // Example: "/a//b" → ["", "a", "", "b"]
        String[] components = path.split("/");

        // Iterate through each path component to process and simplify
        for (String component : components) {
            // Skip empty strings and current directory markers (".")
            // This handles cases like "//" or "./"
            if (component.isEmpty() || component.equals(".")) {
                continue;
            }

            // Handle parent directory navigation ("..")
            // If the stack is not empty, remove the last directory (go up one level)
            if (component.equals("..")) {
                // Prevents going above root directory
                if (!stack.isEmpty()) {
                    stack.pollLast();
                }
                continue;
            }

            // Add valid directory names to the stack
            // This includes special directory names like "..."
            stack.offerLast(component);
        }

        // Reconstruct the simplified canonical path
        // Start with a root slash "/"
        StringBuilder result = new StringBuilder("/");

        // Append each directory from the stack
        for (String dir : stack) {
            result.append(dir).append("/");
        }

        // Remove the trailing slash for non-root paths
        // Ensures the path follows canonical format
        if (result.length() > 1) {
            result.deleteCharAt(result.length() - 1);
        }

        return result.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: Basic path with trailing slash
        String test1 = "/home/";
        System.out.println("Result 1: " + simplifyPath(test1));

        // Test Case 2: Multiple consecutive slashes
        String test2 = "/home//foo/";
        System.out.println("Result 2: " + simplifyPath(test2));

        // Test Case 3: Path with parent directory navigation
        String test3 = "/home/user/Documents/../Pictures";
        System.out.println("Result 3: " + simplifyPath(test3));

        // Test Case 4: Trying to navigate above root directory
        String test4 = "/../";
        System.out.println("Result 4: " + simplifyPath(test4));

        // Test Case 5: Complex path with multiple scenarios
        String test5 = "/.../a/../b/c/../d/./";;
        System.out.println("Result 5: " + simplifyPath(test5));
    }
}

```





