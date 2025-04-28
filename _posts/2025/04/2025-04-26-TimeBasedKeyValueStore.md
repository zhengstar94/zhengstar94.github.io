---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "981. Time Based Key-Value Store"
date: "2025-04-26"
tags: Medium
categories:
  - "LeetCode BinarySearch"
---


- Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.
- Implement the `TimeMap` class:
  - `TimeMap()` Initializes the object of the data structure.
  - `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
  - `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

**Example 1**

```
Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[ [], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5] ]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"
```

## Method 1

```tex
【O(set O(1), get O(logn)) time | O(n) space】
```

```java
package Leetcode.BinarySearch;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/04/26
 */
public class TimeMap {
    /**
     * Custom Pair class to store timestamp-value pairs
     * @param <K> Type of key (timestamp)
     * @param <V> Type of value (string)
     */
    class Pair<K, V> {
        private K key;
        private V value;

        /**
         * Constructor to create a new Pair
         * @param key The timestamp
         * @param value The value associated with the timestamp
         */
        public Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }

        public K getKey() {
            return key;
        }

        public V getValue() {
            return value;
        }
    }

    // HashMap to store key to timestamp-value pairs mapping
    private Map<String, List<Pair<Integer, String>>> map;

    /**
     * Initialize TimeMap data structure
     */
    public TimeMap() {
        map = new HashMap<>();
    }

    /**
     * Stores the key, value pair at the given timestamp
     * @param key The key to store
     * @param value The value to store
     * @param timestamp The timestamp at which to store the key-value pair
     */
    public void set(String key, String value, int timestamp) {
        map.computeIfAbsent(key, k -> new ArrayList<>())
                .add(new Pair<>(timestamp, value));
    }

    /**
     * Gets the value associated with the key at the maximum timestamp less than or equal to the given timestamp
     * @param key The key to look up
     * @param timestamp The timestamp to search for
     * @return The value associated with the key at the greatest timestamp <= given timestamp, or "" if not found
     */
    public String get(String key, int timestamp) {
        if (!map.containsKey(key)) {
            return "";
        }

        List<Pair<Integer, String>> list = map.get(key);
        return binarySearch(list, timestamp);
    }

    /**
     * Binary search to find the value at the greatest timestamp <= target timestamp
     * @param list List of timestamp-value pairs
     * @param timestamp Target timestamp
     * @return Value at the greatest timestamp <= target timestamp, or "" if not found
     */
    private String binarySearch(List<Pair<Integer, String>> list, int timestamp) {
        int left = 0;
        int right = list.size() - 1;

        // If smallest timestamp is greater than target, return empty string
        if (list.get(left).getKey() > timestamp) {
            return "";
        }

        // Binary search to find the largest timestamp <= target
        while (left < right) {
            int mid = left + (right - left + 1) / 2;
            if (list.get(mid).getKey() <= timestamp) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }

        return list.get(left).getValue();
    }
    
    public static void main(String[] args) {
        // Test case 1: Basic operations
        TimeMap timeMap = new TimeMap();
        timeMap.set("foo", "bar", 1);
        System.out.println(timeMap.get("foo", 1));  // Returns "bar"
        System.out.println(timeMap.get("foo", 3));  // Returns "bar"
        timeMap.set("foo", "bar2", 4);
        System.out.println(timeMap.get("foo", 4));  // Returns "bar2"
        System.out.println(timeMap.get("foo", 5));  // Returns "bar2"

        // Test case 2: Multiple timestamps for same key
        TimeMap timeMap2 = new TimeMap();
        timeMap2.set("love", "high", 10);
        timeMap2.set("love", "low", 20);
        System.out.println(timeMap2.get("love", 5));   // Returns ""
        System.out.println(timeMap2.get("love", 10));  // Returns "high"
        System.out.println(timeMap2.get("love", 15));  // Returns "high"
        System.out.println(timeMap2.get("love", 20));  // Returns "low"
        System.out.println(timeMap2.get("love", 25));  // Returns "low"
    }
}

```





