# LRU LinkedList

##  Concept
Implementation of the LRU (least recently used) cache to manage a set of key-value pairs, in which up to maxSize key-value pairs are saved.

## Steps

1. ListNode Class：</br>
   This is an inner class that represents nodes in a two-way linked list. Each node contains a key (key), a value (value), a precursor node (prev) and a successor node (next).
2. LRUCache Class：</br>
   This is the actual cache class that is used to manage the LRU cache.
   - `cache`：This is a mapping where the key is a string and the value is the corresponding node, which is used to quickly find the node in the cache.
   - `maxSize`：Specifies the maximum capacity of the cache.
   - `head` & `tail`：The Sentinel node serves as the head and tail of the linked list, respectively.
3. Constructor function LRUCache(int maxSize)：</br>
   Initialize the LRU cache. Create an empty `cache` map and set the connection between the head and tail sentinel nodes.
4. insertKeyValuePair function：</br>
   If the key already exists in the cache, update the value of the corresponding node and move the node to the header of the linked list (indicating that it has been recently used). If the key does not exist, check that the cache is full, if so, remove the tail node (longest unused), and then insert a new node into the header of the linked list. In any case, the cache mapping needs to be updated.
5. getValueFromKey function：</br>
   If the key exists in the cache, the value of the corresponding node is returned and the node is moved to the header of the linked list. If the key does not exist, return-1 (or you can throw an exception to indicate that it is not found).
6. getMostRecentKey function：</br>
   Returns the most recently used key (that is, the key of the header node of the linked list), or an empty string if the cache is empty (or throws an exception to indicate that the cache is empty).
7. addToHead function：</br>
   Insert a node at the head of the linked list.
8. removeNode function：</br>
   Removes the specified node from the linked list.
9. moveToHead function：</br>
   Move a node to the head of the linked list.
10. removeTail function：</br>
    Remove the tail node of the linked list and return to it.

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

1. **Initialization LinkedList**<br>
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/22.png)

2. **Add new node4 to head, The node4 node points to head.next and the node4.prev points to head, `node.next = head.next`, `node.prev = head`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/23.png)

3. **Head.next.prev points to the node4 node, `head.next.prev = node`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/24.png)

4. **Head next points to the node4 node, `head.next = node`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/25.png)

### removeNode
```java
private void removeNode(ListNode node) {
        node.prev.next = node.next;     // Point the next node of the previous node of the current node to the next node of the current node
        node.next.prev = node.prev;     // Point the previous node of the next node of the current node to the previous node of the current node
        }
```

1. **Initialization LinkedList**<br>
   ![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/26.png)

2. **Remove the node2 node, the next node of the previous node of node2, and point to the next node of node2, `node.prev.next = node.next`**<br>
   ![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/27.png)

3. **The prev node of the next node of node2 points to the prev node of node2, `node.next.prev = node.prev`**<br>
   ![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/28.png)

## Code
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