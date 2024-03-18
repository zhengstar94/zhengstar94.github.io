# LeetCode 23. Merge k Sorted Lists

- You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.
- *Merge all the linked-lists into one sorted linked-list and return it.*

**Example 1**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2**

```
Input: lists = []
Output: []
```

**Example 3**

```
Input: lists = [[]]
Output: []
```

## Method 1

```tex
【O(nlog(k))time∣O(log(k))space】
```

```java
package Leetcode.LinkList;

/**
 * @author zhengstars
 * @date 2024/03/18
 */
public class MergeKSortedLists {
    public static ListNode mergeKLists(ListNode[] lists) {
        // If lists is null or empty, return null
        if(lists == null || lists.length == 0){
            return null;
        }
        int length = lists.length;
        // Start the recursive merging process
        return mergeLists(lists, 0, length - 1);
    }
    
    private static ListNode mergeLists(ListNode[] lists, int start, int end) {
        // If start index is bigger than end index, return null
        if(start > end){
            return null;
        }

        // If start index and end index are the same, we have found a sublist
        if(start == end){
            return lists[start];
        }

        // Divide the lists into two sublists
        int mid = start + (end - start)/2;
        ListNode left = mergeLists(lists, start, mid);
        ListNode right = mergeLists(lists, mid + 1, end);
        // Merge the two sorted sublists and return
        return merge(left,right);
    }

    private static ListNode merge(ListNode left, ListNode right) {
        // If one of the lists is null, return the other
        if(left == null){
            return right;
        }
        if(right == null){
            return left;
        }

        ListNode dummyNode = new ListNode(0);
        ListNode tail = dummyNode;

        // Merge the nodes in the two lists into one list based on their values
        while (left != null && right != null){
            if(left.val <= right.val){
                tail.next = left;
                left = left.next;
            }else{
                tail.next = right;
                right = right.next;
            }
            tail = tail.next;
        }

        // If there are remaining nodes in either list, append them to the result list
        if(null == left){
            tail.next = right;
        } else {
            tail.next = left;
        }
        return dummyNode.next;  // return the merged sorted list
    }

    public static void main(String[] args){
        // Example 1
        ListNode node1 = new ListNode(1);
        node1.next = new ListNode(4);
        node1.next.next = new ListNode(5);

        ListNode node2 = new ListNode(1);
        node2.next = new ListNode(3);
        node2.next.next = new ListNode(4);

        ListNode node3 = new ListNode(2);
        node3.next = new ListNode(6);

        ListNode[] lists = new ListNode[]{node1, node2, node3};
        System.out.println(mergeKLists(lists)); // expect output: [1,1,2,3,4,4,5,6]

        // Example 2
        ListNode[] lists2 = new ListNode[]{};
        System.out.println(mergeKLists(lists2)); // expect output: []

        // Example 3
        ListNode[] lists3 = new ListNode[]{null};
        System.out.println(mergeKLists(lists3)); // expect output: []
    }
}

```

