---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "LRU LinkedList"
date: "2023-08-19"
categories: 
  - "Data Structure"
---

# LRU LinkedList

##  Concept
Implementation of the LRU (least recently used) cache to manage a set of key-value pairs, in which up to maxSize key-value pairs are saved.

## Steps

- ListNode Class：

  - This is an inner class that represents nodes in a two-way linked list. Each node contains a key (key), a value (value), a precursor node (prev) and a successor node (next).

- LRUCache Class：

   This is the actual cache class that is used to manage the LRU cache.

   - `cache`：This is a mapping where the key is a string and the value is the corresponding node, which is used to quickly find the node in the cache.
   - `maxSize`：Specifies the maximum capacity of the cache.
   - `head` & `tail`：The Sentinel node serves as the head and tail of the linked list, respectively.

- Constructor function LRUCache(int maxSize)：

  - Initialize the LRU cache. Create an empty `cache` map and set the connection between the head and tail sentinel nodes.

- insertKeyValuePair function：

  - If the key already exists in the cache, update the value of the corresponding node and move the node to the header of the linked list (indicating that it has been recently used). If the key does not exist, check that the cache is full, if so, remove the tail node (longest unused), and then insert a new node into the header of the linked list. In any case, the cache mapping needs to be updated.

- getValueFromKey function：

  - If the key exists in the cache, the value of the corresponding node is returned and the node is moved to the header of the linked list. If the key does not exist, return-1 (or you can throw an exception to indicate that it is not found).

- getMostRecentKey function：

  - Returns the most recently used key (that is, the key of the header node of the linked list), or an empty string if the cache is empty (or throws an exception to indicate that the cache is empty).

- addToHead function：

  - Insert a node at the head of the linked list.

- removeNode function：

  - Removes the specified node from the linked list.

- moveToHead function：

  - Move a node to the head of the linked list.

- removeTail function：

  - Remove the tail node of the linked list and return to it.


## Detailed method interpretation

### addToHead
```java
private void addToHead(ListNode node) {
    node.next = head.next;     // Point the next node of the new node to the next node of the current header node
    node.prev = head;          // Point the previous node of the new node to the header node
    head.next.prev = node;     // Point the previous node of the current header node to the new node
    head.next = node;          // Point the next node of the head node to the new node to complete the insert operation
}
```

1.**Initialization LinkedList**<br>

{% include figure.liquid loading="eager" path="assets/img/2023/08/22.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2.**Add new node4 to head, The node4 node points to head.next and the node4.prev points to head, `node.next = head.next`, `node.prev = head`**

{% include figure.liquid loading="eager" path="assets/img/2023/08/23.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.**Head.next.prev points to the node4 node, `head.next.prev = node`**

{% include figure.liquid loading="eager" path="assets/img/2023/08/24.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

4.**Head next points to the node4 node, `head.next = node`**

{% include figure.liquid loading="eager" path="assets/img/2023/08/25.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### removeNode
```java
private void removeNode(ListNode node) {
        node.prev.next = node.next;     // Point the next node of the previous node of the current node to the next node of the current node
        node.next.prev = node.prev;     // Point the previous node of the next node of the current node to the previous node of the current node
        }
```

1.**Initialization LinkedList**<br>

{% include figure.liquid loading="eager" path="assets/img/2023/08/26.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2.**Remove the node2 node, the next node of the previous node of node2, and point to the next node of node2, `node.prev.next = node.next`**<br>

{% include figure.liquid loading="eager" path="assets/img/2023/08/27.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.**The prev node of the next node of node2 points to the prev node of node2, `node.next.prev = node.prev`**<br>

{% include figure.liquid loading="eager" path="assets/img/2023/08/28.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## Code

### Method 1
```java
package LinkedLists;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengstars
 * @date 2023/06/25
 */
public class LRUCache {
    class ListNode {
        String key;
        int value;
        ListNode prev;
        ListNode next;

        ListNode(String key, int value) {
            this.key = key;
            this.value = value;
            prev = null;
            next = null;
        }
    }

    private Map<String, ListNode> cache;
    private int maxSize;
    private ListNode head;
    private ListNode tail;

    public LRUCache(int maxSize) {
        this.maxSize = maxSize;
        cache = new HashMap<>();
        head = new ListNode("", 0);
        tail = new ListNode("", 0);
        head.next = tail;
        tail.prev = head;
    }

    public void insertKeyValuePair(String key, int value) {
        if (cache.containsKey(key)) {
            ListNode node = cache.get(key);
            node.value = value;
            moveToHead(node);
        } else {
            if (cache.size() == maxSize) {
                ListNode removedNode = removeTail();
                cache.remove(removedNode.key);
            }
            ListNode newNode = new ListNode(key, value);
            cache.put(key, newNode);
            addToHead(newNode);
        }
    }

    public int getValueFromKey(String key) {
        if (cache.containsKey(key)) {
            ListNode node = cache.get(key);
            moveToHead(node);
            return node.value;
        }
        return -1; // or throw an exception to indicate key not found
    }

    public String getMostRecentKey() {
        if (head.next != tail) {
            return head.next.key;
        }
        return ""; // or throw an exception to indicate cache is empty
    }

    private void addToHead(ListNode node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(ListNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void moveToHead(ListNode node) {
        removeNode(node);
        addToHead(node);
    }

    private ListNode removeTail() {
        ListNode removedNode = tail.prev;
        removeNode(removedNode);
        return removedNode;
    }

    public static void main(String[] args) {
        LRUCache cache = new LRUCache(3);

        cache.insertKeyValuePair("b", 2);
        cache.insertKeyValuePair("a", 1);
        cache.insertKeyValuePair("c", 3);

        cache.getMostRecentKey(); // Output: "c"

        cache.getValueFromKey("a"); // Output: 1

        cache.getMostRecentKey(); // Output: "a"

        cache.insertKeyValuePair("d", 4);

        cache.getValueFromKey("b"); // Output: -1

        cache.insertKeyValuePair("a", 5);

        cache.getValueFromKey("a"); // Output: 5

    }
}
```

### Method 2
```java

import java.util.LinkedHashMap;
import java.util.Map;

public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true); // true: access-order, false: insertion-order
        this.capacity = capacity;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }

    public V get(K key) {
        return super.getOrDefault(key, null);
    }

    public void put(K key, V value) {
        super.put(key, value);
    }

    public static void main(String[] args) {
        LRUCache<Integer, String> cache = new LRUCache<>(3);

        cache.put(1, "A");
        cache.put(2, "B");
        cache.put(3, "C");

        System.out.println(cache); // 输出: {1=A, 2=B, 3=C}

        cache.get(1);  // 访问key为1的元素
        cache.put(4, "D"); // 插入新元素

        System.out.println(cache); // 输出: {2=B, 1=A, 4=D}
    }
}

```