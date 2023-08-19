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

1. Initialization LinkedList<br>
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/22.png)

