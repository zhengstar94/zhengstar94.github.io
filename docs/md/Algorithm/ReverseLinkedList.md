# Reverse Linked List

## Solution (Traverse the linked list, pointing the next of each node to the previous node)

1. Save the next of the current node, `next = curr.next`
2. Point the next of the current node to the previous node, `curr.next = prev`
3. Move the forward pointer back to `prev = curr`
4. Move the current pointer back to `curr = next`

## Step

1. **Initial list and pre node**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/1.png)

2. **Initial list and next node, `next = curr.next`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/2.png)

3. **The current.next points to the pre node, `curr.next = prev`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/3.png)

4. **The pre node points to the current node, `prev = curr`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/4.png)

5. **The current node points to the next node, `curr = nex`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/5.png)

6. **The next node moves to the current.next, `next = curr.next`**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/6.png)

7. **Loop traversal**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/7.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/8.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/9.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/10.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/11.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/12.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/13.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/14.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/15.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/16.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/17.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/18.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/19.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/20.png)
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/19/21.png)


## Code

```java
package LinkedLists;

/**
 * @author zhengstars
 * @date 2023/08/19
 */
public class ReverseLinkedList {
    public static class ListNode {
        int value;
        ListNode next;

        public ListNode(int value) {
            this.value = value;
            this.next = null;
        }
    }

    public static ListNode reverseLinkedList(ListNode head) {
        ListNode prev = null;

        ListNode curr = head;

        ListNode next = null;

        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;

        }
        return prev;
    }
}
```

