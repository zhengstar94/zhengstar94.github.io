# Morris traversal of binary Tree

## Conception
1. Nodes without left subtrees will only arrive once, and nodes with left subtrees will arrive twice.
2. The use of the tree's leaf nodes around the child is empty (a large number of free pointers of the tree), so as to achieve a rapid reduction in space overhead.

### Explain
> The node uses the right pointer direction of the rightmost node on the left tree to mark whether it is the first or second time.<br>
>   1. If this is the first time you mark yourself, move to the left and use the same strategy to execute the subtree (left tree).
>   2. If it is the second time to mark yourself, after the recovery is complete, go to the right subtree to repeat the behavior (1,2).

1. The node has no left tree and the node only arrives once.
   - If the rightmost node of the left subtree of a node points to empty, the node must be coming for the first time.
2. The node has a left tree and the node arrives twice.
   - If the rightmost node of the left subtree of the current node points to itself, the node must be coming for the second time.


## Details
Suppose the current node cur, and the cur comes to the header node at the beginning.
1. If cur does not have a left child, cur moves to the right, `cur = cur.right`.
2. If cur has a left child, find the rightmost node morrisRight of the left subtree.
   1. If the right pointer of morrisRight points to null, let it point to cur, and then cur moves to the left, `cur = cur.left`.
   2. If the right pointer of morrisRight points to cur, let it point to null, and then cur moves to the right, `cur = cur.right`.
3. Cur is null and traversing stops.

## Logic

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/20/1.png)

**Recursive order: ①②②②①③③③① [System stack]**

Pre-order (the result of the first occurrence): ①②③ <br>
Middle order (the result of the second occurrence): ②①③ <br>
Post-order (the result of the third occurrence): ②③① <br>

### Morris traversal highly simulates recursive behavior
1. There is no left subtree node and can only be reached once.
2. There are left subtree nodes, which can reach 2 times.
3. The node cannot be reached 3 times.
> Morris uses `a node to point the right pointer to the left tree` to solve this problem.

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/20/2.png)

**a, b, c, d, e, f, `d`, `a`, g, h, i, `h`, k, `g`**


## Code Content

> Suppose the current node cur, and the cur comes to the header node at the beginning.
> 1. If cur does not have a left child, cur moves to the right, `cur = cur.right`.
> 2. If cur has a left child, find the rightmost node morrisRight of the left subtree.
>    1. If the right pointer of morrisRight points to null, let it point to cur, and then cur moves to the left, `cur = cur.left`.
>    2. If the right pointer of morrisRight points to cur, let it point to null, and then cur moves to the right, `cur = cur.right`.
> 3. Cur is null and traversing stops。


### General Code

```java
package Algorithm1763;

import NewArray.BSTTraversal4;

/**
 * @author zhengstars
 * @date 2023/08/20
 */
public class Morris {

    public static class Node {
        int val;
        Node left;
        Node right;
        Node() {}
        Node(int val) { this.val = val; }
        Node(int val, Node left, Node right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }
    }
    
    public static void morris(Node head){
        if(head == null){
            return;
        }

        Node cur = head;
        Node morrisRight = null;

        while (cur != null){
            // morrisRight is cur's left child
            morrisRight = cur.left;
            if(morrisRight != null){
                while (morrisRight.right != null && morrisRight.right != cur){
                    morrisRight = morrisRight.right;
                }

                //After while execution, morrisRight comes to the position of the rightmost child in the left subtree of cur.

                if(morrisRight.right == null){
                    // Come to cur for the first time
                    morrisRight.right = cur;
                    cur = cur.left;
                    continue;
                }else{
                    // Come to cur for the second time, morrisRight.right == cur
                    morrisRight.right = null;
                }
            }
            cur = cur.right;
        }
    }
}
```

**Example**

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2023/08/20/3.png) <br>

Morris traversal: **①②④②⑤①③⑥③⑦**

- Pre-order (the result of the first occurrence): ①②④⑤③⑥⑦ <br>
- Middle order (the result of the second occurrence): ④②⑤①⑥③⑦ <br>