---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "981.Time Based Key-Value Store"
date: "2024-01-04"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 981. Time Based Key-Value Store [Easy]

- Create a timebased key-value store class `TimeMap`, that supports two operations.

  1. `set(string key, string value, int timestamp)`

  - Stores the `key` and `value`, along with the given `timestamp`.

  2. `get(string key, int timestamp)`

  - Returns a value such that `set(key, value, timestamp_prev)` was called previously, with `timestamp_prev <= timestamp`.
  - If there are multiple such values, it returns the one with the largest `timestamp_prev`.
  - If there are no values, it returns the empty string (`""`).

**Example 1**

```
Input: inputs = ["TimeMap","set","set","get","get","get","get","get"], inputs = [[],["love","high",10],["love","low",20],["love",5],["love",10],["love",15],["love",20],["love",25]]
Output: [null,null,null,"","high","high","low","low"]
```

**Example 2**

```
Input: inputs = ["TimeMap","set","set","get","get","get","get","get"], inputs = [[],["love","high",10],["love","low",20],["love",5],["love",10],["love",15],["love",20],["love",25]]
Output: [null,null,null,"","high","high","low","low"]
```



**Note:**

1. All key/value strings are lowercase.
2. All key/value strings have length in the range `[1, 100]`
3. The `timestamps` for all `TimeMap.set` operations are strictly increasing.
4. `1 <= timestamp <= 10^7`
5. `TimeMap.set` and `TimeMap.get` functions will be called a total of `120000` times (combined) per test case.

## Method 1

```tex
【O(log(n))time∣O(m)space】
```

```java
package Leetcode.BinarySearch;

import java.util.HashMap;
import java.util.Map;
import java.util.TreeMap;

/**
 * Time-based value store.
 * @author zhengstars
 * @date 2024/01/21
 */
public class TimeBasedKey {
  // Node class to store the value and its corresponding timestamp.
  class Node {
    String value;
    int timestamp;

    // Constructor for Node.
    Node(String value, int timestamp) {
      this.value = value;
      this.timestamp = timestamp;
    }
  }

  // An outer map to store String key with a TreeMap as value.
  // The TreeMap stores timestamp as key (sorted) and String value.
  Map<String, TreeMap<Integer, String>> map;

  // Initialize the outer map here.
  public TimeBasedKey() {
    map = new HashMap<>();
  }

  // Method to set the value with its timestamp.
  // If the key is not present, it creates a new TreeMap.
  // Finally, it adds timestamp and value to the key's TreeMap.
  public void set(String key, String value, int timestamp) {
    if (!map.containsKey(key)) {
      map.put(key, new TreeMap<>());
    }
    map.get(key).put(timestamp, value);
  }

  // Method to get the value from given key and timestamp.
  // It returns the most recent value till the timestamp.
  // If the key is not present, it returns an empty string.
  public String get(String key, int timestamp) {
    if (!map.containsKey(key)) {
      return "";
    }

    //Get the TreeMap of given key
    TreeMap<Integer, String> treeMap = map.get(key);
    //Get the most recent timestamp till the given timestamp.
    Integer t = treeMap.floorKey(timestamp);
    //If no value is present till the given timestamp, return empty string.
    if (t==null) {
      return "";
    }

    //Return the value of recent timestamp.
    return treeMap.get(t);
  }

  public static void main(String[] args) {
    TimeBasedKey kv = new TimeBasedKey();
    kv.set("foo", "bar", 1); // Store the key "foo" with value "bar" and timestamp = 1.
    System.out.println(kv.get("foo", 1));  // Output "bar", as the recent value till timestamp 1 is "bar".
    System.out.println(kv.get("foo", 3));  // Output "bar", as there is no later value till timestamp 3.
    kv.set("foo", "bar2", 4);  // Store the key "foo" with value "bar2" and timestamp = 4.
    System.out.println(kv.get("foo", 4));  // Output "bar2", the recent value till timestamp 4 is "bar2".
    System.out.println(kv.get("foo", 5));  // Output "bar2", as there is no later value till timestamp 5.
  }
}

```















