---
toc:
beginning: true
giscus_comments: true
layout: post
title: "1233. Remove Sub-Folders from the Filesystem"
date: "2025-07-19"
tags: Medium
categories:
- "LeetCode Greedy"
---


- Given a list of folders `folder`, return *the folders after removing all **sub-folders** in those folders*. You may return the answer in **any order**.
- If a `folder[i]` is located within another `folder[j]`, it is called a **sub-folder** of it. A sub-folder of `folder[j]` must start with `folder[j]`, followed by a `"/"`. For example, `"/a/b"` is a sub-folder of `"/a"`, but `"/b"` is not a sub-folder of `"/a/b/c"`.
- The format of a path is one or more concatenated strings of the form: `'/'` followed by one or more lowercase English letters.
  - For example, `"/leetcode"` and `"/leetcode/problems"` are valid paths while an empty string and `"/"` are not.

**Example 1**

```
Input: folder = ["/a","/a/b","/c/d","/c/d/e","/c/f"]
Output: ["/a","/c/d","/c/f"]
Explanation: Folders "/a/b" is a subfolder of "/a" and "/c/d/e" is inside of folder "/c/d" in our filesystem.
```

**Example 2**

```
Input: folder = ["/a","/a/b/c","/a/b/d"]
Output: ["/a"]
Explanation: Folders "/a/b/c" and "/a/b/d" will be removed because they are subfolders of "/a".
```

**Example 3**

```
Input: folder = ["/a/b/c","/a/b/ca","/a/b/d"]
Output: ["/a/b/c","/a/b/ca","/a/b/d"]
```

## Method 1

```tex
【O(n * m) time | O(n * m) space】
```

```java
package Leetcode.Greedy;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @Author zhengxingxing
 * @Date 2025/07/19
 */
public class RemoveSubFoldersFromTheFilesystem {
    public static List<String> removeSubfolders(String[] folder) {
        // Step 1: Sort the folder paths lexicographically.
        // This ensures that any parent folder comes before its subfolders.
        Arrays.sort(folder);

        List<String> result = new ArrayList<>();

        // This variable keeps track of the last added parent folder.
        String prev = "";

        // Step 2: Iterate through each folder path.
        for (String path : folder) {
            // Key logic:
            // If 'prev' is empty, this is the first folder, so add it directly.
            // Otherwise, check if the current path is NOT a subfolder of 'prev'.
            // To be a subfolder, the path must start with 'prev' followed by a '/'.
            // Example: prev = "/a", path = "/a/b" -> "/a/b".startsWith("/a/") == true (subfolder)
            //          prev = "/a", path = "/ab"  -> "/ab".startsWith("/a/") == false (not a subfolder)
            if (prev.isEmpty() || !path.startsWith(prev + "/")) {
                // If not a subfolder, add to result and update 'prev'
                result.add(path);
                prev = path;
            }
            // If it is a subfolder (startsWith(prev + "/")), skip it.
        }

        // Step 3: Return the filtered list of folders.
        return result;
    }

    public static void main(String[] args) {
        // Test case 1: Some folders are subfolders of others
        String[] test1 = {"/a","/a/b","/c/d","/c/d/e","/c/f"};
        // Expected output: ["/a", "/c/d", "/c/f"]

        // Test case 2: All folders are subfolders of the first one
        String[] test2 = {"/a","/a/b/c","/a/b/d"};
        // Expected output: ["/a"]

        // Test case 3: No folder is a subfolder of another
        String[] test3 = {"/a/b/c","/a/b/ca","/a/b/d"};
        // Expected output: ["/a/b/c", "/a/b/ca", "/a/b/d"]

        System.out.println(removeSubfolders(test1)); // Output: ["/a", "/c/d", "/c/f"]
        System.out.println(removeSubfolders(test2)); // Output: ["/a"]
        System.out.println(removeSubfolders(test3)); // Output: ["/a/b/c", "/a/b/ca", "/a/b/d"]
    }
}

```





