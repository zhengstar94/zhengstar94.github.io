# LeetCode 981. Time Based Key-Value Store

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
 * @author zhengstars
 * @date 2024/01/21
 */
public class TimeBasedKey {
    class Node {
        String value;
        int timestamp;
        Node(String value, int timestamp) {
            this.value = value;
            this.timestamp = timestamp;
        }
    }

    Map<String, TreeMap<Integer, String>> map;

    /** Initialize your data structure here. */
    public TimeBasedKey() {
        map = new HashMap<>();
    }

    public void set(String key, String value, int timestamp) {
        if (!map.containsKey(key)) {
            map.put(key, new TreeMap<>());
        }

        map.get(key).put(timestamp, value);
    }

    public String get(String key, int timestamp) {
        if (!map.containsKey(key)) {
            return "";
        }

        TreeMap<Integer, String> treeMap = map.get(key);
        Integer t = treeMap.floorKey(timestamp);
        if (t==null) {
            return "";
        }

        return treeMap.get(t);
    }

    public static void main(String[] args) {
        TimeBasedKey kv = new TimeBasedKey();
        kv.set("foo", "bar", 1); // store the key "foo" and value "bar" along with timestamp = 1
        System.out.println(kv.get("foo", 1));  // output "bar"
        System.out.println(kv.get("foo", 3));  // output "bar"
        kv.set("foo", "bar2", 4);  // store the key "foo" and value "bar2" along with timestamp = 4
        System.out.println(kv.get("foo", 4));  // output "bar2"
        System.out.println(kv.get("foo", 5));  // output "bar2"
    }
}

```















