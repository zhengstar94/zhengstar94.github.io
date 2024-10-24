---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "339.Nested List Weight Sum"
date: "2024-10-24"
categories:
  - "LeetCode Trees"
---

- Given a nested list of integers, return the sum of all integers in the list weighted by their depth.
- Each element is either an integer, or a list -- whose elements may also be integers or other lists.


**Example 1**

```
Input: [ [ 1,1 ], 2, [ 1,1 ] ]
Output: 10 
Explanation: Four 1's at depth 2, one 2 at depth 1.
```

**Example 2**

```
Input: [ 1, [ 4, [6 ] ] ]
Output: 27 
Explanation: One 1 at depth 1, one 4 at depth 2, and one 6 at depth 3; 1 + 4*2 + 6*3 = 27.
```

## Method 1

```tex
【O(n) time | O(d) space】
```

```java
package Leetcode.Trees;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2024/10/24
 */
public class NestedListWeightSum {
    public static int depthSum(List<NestedInteger> nestedList) {
        // Handle edge cases: if input is null or empty
        if(nestedList == null || nestedList.size() == 0){
            return 0;
        }
        // Start DFS with depth 1
        return dfs(nestedList, 1);
    }

    /**
     * Depth-First Search helper method to recursively calculate the weighted sum
     * @param list current list being processed
     * @param depth current depth in the nested structure
     * @return weighted sum of all integers in this list and its sublists
     */
    private static int dfs(List<NestedInteger> list, int depth) {
        int sum = 0;
        // Iterate through each element in the current list
        for (NestedInteger nested: list) {
            if(nested.isInteger()){
                // If element is an integer, multiply it by current depth
                sum += nested.getInteger() * depth;
            }else{
                // If element is a list, recurse deeper with increased depth
                sum += dfs(nested.getList(), depth + 1);
            }
        }
        return sum;
    }

    public static void main(String[] args) {
        // Test Case 1: [ [ 1,1 ] , 2, [1,1 ] ]
        // Expected output: 10 (four 1's at depth 2, one 2 at depth 1)
        List<NestedInteger> test1 = new ArrayList<>();

        // Create first [1,1]
        List<NestedInteger> subList1 = new ArrayList<>();
        subList1.add(new NestedIntegerImpl(1));    // First 1 at depth 2
        subList1.add(new NestedIntegerImpl(1));    // Second 1 at depth 2
        test1.add(new NestedIntegerImpl(subList1));

        // Add single integer 2
        test1.add(new NestedIntegerImpl(2));       // 2 at depth 1

        // Create second [1,1]
        List<NestedInteger> subList2 = new ArrayList<>();
        subList2.add(new NestedIntegerImpl(1));    // Third 1 at depth 2
        subList2.add(new NestedIntegerImpl(1));    // Fourth 1 at depth 2
        test1.add(new NestedIntegerImpl(subList2));

        System.out.println("Test case 1 result: " + depthSum(test1)); // Should output 10

        // Test Case 2: [1, [ 4, [ 6 ] ] ]
        // Expected output: 27 (1 at depth 1, 4 at depth 2, 6 at depth 3)
        List<NestedInteger> test2 = new ArrayList<>();
        test2.add(new NestedIntegerImpl(1));       // 1 at depth 1

        List<NestedInteger> subList3 = new ArrayList<>();
        subList3.add(new NestedIntegerImpl(4));    // 4 at depth 2

        List<NestedInteger> subList4 = new ArrayList<>();
        subList4.add(new NestedIntegerImpl(6));    // 6 at depth 3
        subList3.add(new NestedIntegerImpl(subList4));
        test2.add(new NestedIntegerImpl(subList3));

        System.out.println("Test case 2 result: " + depthSum(test2)); // Should output 27
    }

}

class NestedIntegerImpl implements NestedInteger {
    private Integer value;                  // Holds the integer value if this is a single number
    private List<NestedInteger> list;       // Holds the list if this is a nested list

    /**
     * Constructor for integer value
     * @param value the integer to store
     */
    public NestedIntegerImpl(int value) {
        this.value = value;
        this.list = null;
    }

    /**
     * Constructor for nested list
     * @param list the list of NestedIntegers to store
     */
    public NestedIntegerImpl(List<NestedInteger> list) {
        this.list = list;
        this.value = null;
    }

    @Override
    public boolean isInteger() {
        return value != null;              // Returns true if this holds an integer
    }

    @Override
    public Integer getInteger() {
        return value;                      // Returns the integer value or null
    }

    @Override
    public List<NestedInteger> getList() {
        return list == null ? new ArrayList<>() : list;  // Returns the list or empty list
    }
}

/**
 * Interface defining the structure for nested integers
 * Each NestedInteger can be either an integer or a list of NestedIntegers
 */
interface NestedInteger {
    // Returns true if this NestedInteger holds a single integer, rather than a nested list
    public boolean isInteger();

    // Returns the single integer that this NestedInteger holds if it holds a single integer
    // Returns null if this NestedInteger holds a nested list
    public Integer getInteger();

    // Returns the nested list that this NestedInteger holds if it holds a nested list
    // Returns an empty list if this NestedInteger holds a single integer
    public List<NestedInteger> getList();
}

```