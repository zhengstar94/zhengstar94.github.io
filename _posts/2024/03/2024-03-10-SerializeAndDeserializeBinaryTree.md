---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "297.Serialize and Deserialize Binary Tree"
date: "2024-03-10"
categories:
  - "LeetCode Trees"
---

# 297. Serialize and Deserialize Binary Tree

- Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

  Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

  **Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://support.leetcode.com/hc/en-us/articles/360011883654-What-does-1-null-2-3-mean-in-binary-tree-representation-). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example 1**

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

**Example 2**

```
Input: root = []
Output: []
```

**Example 3**

```
Input: root = [1]
Output: [1]
```

**Example 4**

```
Input: root = [1,2]
Output: [1,2]
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Trees;

import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

/**
 * @author zhengstars
 * @date 2024/05/05
 */
public class SerializeAndDeserializeBinaryTree {
    public String serialize(TreeNode root) {
        return serializeHelper(root, "");
    }

    private String serializeHelper(TreeNode root, String str){
        if (root == null) {
            // add null, to the string when a leaf node is encountered.
            str += "null,";
        } else {
            // add root value to the string
            str += String.valueOf(root.val) + ",";
            // serialize the left subtree
            str = serializeHelper(root.left, str);
            // serialize the right subtree
            str = serializeHelper(root.right, str);
        }
        return str;
    }

    // Deserializes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] dataArr = data.split(",");
        Queue<String> dataQueue = new LinkedList<>(Arrays.asList(dataArr));
        return deserializeHelper(dataQueue);
    }

    private TreeNode deserializeHelper(Queue<String> dataQueue) {
        // remove one node from the queue
        String current = dataQueue.remove(); 
        if (current.equals("null")) {
            return null;
        }

        // create a new node
        TreeNode node = new TreeNode(Integer.valueOf(current));
        // build the left subtree
        node.left = deserializeHelper(dataQueue);
        // build the right subtree
        node.right = deserializeHelper(dataQueue); 

        return node;
    }

    public static void main(String[] args) {
        SerializeAndDeserializeBinaryTree ser = new SerializeAndDeserializeBinaryTree();
        SerializeAndDeserializeBinaryTree deser = new SerializeAndDeserializeBinaryTree();

        TreeNode testcase1 = new TreeNode(1, new TreeNode(2), new TreeNode(3, new TreeNode(4), new TreeNode(5)));
        String tree = ser.serialize(testcase1);
        TreeNode ans = deser.deserialize(tree);
        String output = ser.serialize(ans);
        System.out.println(output);

        TreeNode testcase2 = new TreeNode();
        String tree2 = ser.serialize(testcase2);
        TreeNode ans2 = deser.deserialize(tree2);
        String output2 = ser.serialize(ans2);
        System.out.println(output2);

        TreeNode testcase3 = new TreeNode(1);
        String tree3 = ser.serialize(testcase3);
        TreeNode ans3 = deser.deserialize(tree3);
        String output3 = ser.serialize(ans3);
        System.out.println(output3);

        TreeNode testcase4 = new TreeNode(1, new TreeNode(2), null);
        String tree4 = ser.serialize(testcase4);
        TreeNode ans4 = deser.deserialize(tree4);
        String output4 = ser.serialize(ans4);
        System.out.println(output4);
    }
}

```

