---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1472. Design Browser History"
date: "2025-02-26"
tags: Medium
categories:
  - "LeetCode Array"
---


- You have a **browser** of one tab where you start on the `homepage` and you can visit another `url`, get back in the history number of `steps` or move forward in the history number of `steps`.
- Implement the `BrowserHistory` class:
  - `BrowserHistory(string homepage)` Initializes the object with the `homepage` of the browser.
  - `void visit(string url)` Visits `url` from the current page. It clears up all the forward history.
  - `string back(int steps)` Move `steps` back in history. If you can only return `x` steps in the history and `steps > x`, you will return only `x` steps. Return the current `url` after moving back in history **at most** `steps`.
  - `string forward(int steps)` Move `steps` forward in history. If you can only forward `x` steps in the history and `steps > x`, you will forward only `x` steps. Return the current `url` after forwarding in history **at most** `steps`.


**Example 1**

```
Input:
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
Output:
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]

Explanation:
BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
browserHistory.visit("google.com");       // You are in "leetcode.com". Visit "google.com"
browserHistory.visit("facebook.com");     // You are in "google.com". Visit "facebook.com"
browserHistory.visit("youtube.com");      // You are in "facebook.com". Visit "youtube.com"
browserHistory.back(1);                   // You are in "youtube.com", move back to "facebook.com" return "facebook.com"
browserHistory.back(1);                   // You are in "facebook.com", move back to "google.com" return "google.com"
browserHistory.forward(1);                // You are in "google.com", move forward to "facebook.com" return "facebook.com"
browserHistory.visit("linkedin.com");     // You are in "facebook.com". Visit "linkedin.com"
browserHistory.forward(2);                // You are in "linkedin.com", you cannot move forward any steps.
browserHistory.back(2);                   // You are in "linkedin.com", move back two steps to "facebook.com" then to "google.com". return "google.com"
browserHistory.back(7);                   // You are in "google.com", you can move back only one step to "leetcode.com". return "leetcode.com"
```

## Method 1

```tex
【O(1) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;

/**
 * @author zhengxingxing
 * @date 2025/02/26
 */
public class BrowserHistory {

    private ArrayList<String> history; // Store browser history
    private int current;              // Current position index
    private int size;                 // Valid history size

    /**
     * Initialize browser history with homepage
     * @param homepage Initial homepage URL
     */
    public BrowserHistory(String homepage) {
        history = new ArrayList<>();
        history.add(homepage);
        current = 0;
        size = 1;
    }

    /**
     * Visit a new URL
     * @param url URL to visit
     */
    public void visit(String url) {
        current++;
        // If current exceeds ArrayList size, add new element
        if (current == history.size()) {
            history.add(url);
        } else {
            history.set(current, url);
        }
        size = current + 1; // Update valid history size
    }

    /**
     * Move back in history
     * @param steps Number of steps to move back
     * @return URL after moving back
     */
    public String back(int steps) {
        current = Math.max(0, current - steps);
        return history.get(current);
    }

    /**
     * Move forward in history
     * @param steps Number of steps to move forward
     * @return URL after moving forward
     */
    public String forward(int steps) {
        current = Math.min(size - 1, current + steps);
        return history.get(current);
    }
    
    public static void main(String[] args) {
        BrowserHistory browserHistory = new BrowserHistory("leetcode.com");
        browserHistory.visit("google.com");
        browserHistory.visit("facebook.com");
        browserHistory.visit("youtube.com");

        System.out.println(browserHistory.back(1));    // Returns "facebook.com"
        System.out.println(browserHistory.back(1));    // Returns "google.com"
        System.out.println(browserHistory.forward(1)); // Returns "facebook.com"

        browserHistory.visit("linkedin.com");
        System.out.println(browserHistory.forward(2)); // Returns "linkedin.com"
        System.out.println(browserHistory.back(2));    // Returns "google.com"
        System.out.println(browserHistory.back(7));    // Returns "leetcode.com"
    }
}

```





