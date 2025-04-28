---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "278. First Bad Version"
date: "2024-11-21"
categories:
  - "LeetCode BinarySearch"
---


- You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.
- Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.
- You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example 1**

```
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

**Example 2**

```
Input: n = 1, bad = 1
Output: 1
```

## Method 1

```tex
【O(logn) time | O(1) space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengxingxing
 * @date 2024/11/21
 */
public class FirstBadVersion extends VersionControl{
    // Reference to the VersionControl instance
    VersionControl versionControl;

    public int firstBadVersion(int n) {
        // Use binary search to find the first bad version
        int left = 1;
        int right = n;

        while (left < right) {
            // Calculate mid point to avoid integer overflow
            int mid = left + (right - left) / 2;

            // If mid is a bad version, search in the left half
            if (versionControl.isBadVersion(mid)) {
                right = mid;
            }
            // If mid is a good version, search in the right half
            else {
                left = mid + 1;
            }
        }

        // Return the first bad version
        return left;
    }
}

class VersionControl {
    // The first bad version number
    private int firstBadVersion;

    // Constructor with specified first bad version
    public VersionControl(int firstBadVersion) {
        this.firstBadVersion = firstBadVersion;
    }

    // Default constructor with first bad version set to 4
    public VersionControl() {
        this.firstBadVersion = 4;
    }

    // API method to check if a version is bad
    public boolean isBadVersion(int version) {
        // Return true if version is greater than or equal to first bad version
        return version >= firstBadVersion;
    }

    // Test method to validate the solution
    public static void main(String[] args) {
        // Test case 1: Bad version is 4
        VersionControl vc1 = new VersionControl(4);
        FirstBadVersion FirstBadVersion1 = new FirstBadVersion();
        FirstBadVersion1.versionControl = vc1;
        System.out.println("Test Case 1 (Bad Version 4): " + FirstBadVersion1.firstBadVersion(5)); // Expected output: 4

        // Test case 2: Bad version is 1
        VersionControl vc2 = new VersionControl(1);
        FirstBadVersion FirstBadVersion2 = new FirstBadVersion();
        FirstBadVersion2.versionControl = vc2;
        System.out.println("Test Case 2 (Bad Version 1): " + FirstBadVersion2.firstBadVersion(1)); // Expected output: 1

        // Test case 3: Bad version is 2
        VersionControl vc3 = new VersionControl(2);
        FirstBadVersion FirstBadVersion3 = new FirstBadVersion();
        FirstBadVersion3.versionControl = vc3;
        System.out.println("Test Case 3 (Bad Version 2): " + FirstBadVersion3.firstBadVersion(5)); // Expected output: 2
    }
}
```





