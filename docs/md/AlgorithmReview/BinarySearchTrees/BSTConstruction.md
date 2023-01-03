# BST Construction [Medium]

- Write a `BST` class for a Binary Search Tree.The class should support:

    - Inserting values with the `insert` method
    - Removing values with the `remove` method; this method should only remove the first instance of a given value.
    - Searching for values with the `contains` method.

- Note that you can't remove values from a single-node tree. In other words,calling the `remove` method on a single-node tree should simply not do anything.

- Each `BST` node has an integer `value`,  a `left` child node,and a `right` child node.A node is said to be a valid `BST` node if and only if it satisfies the BST property: its `value` is strictly greater than the values of every node to its left; its `value` is less than or equal to the values of every node to its right; and its children nodes are either valid `BST` nodes themselves or `None` / `null`.



**Sample Usage**

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/08/29/1.png)

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/08/29/2.png)



## Method 1

```tex
Average:\ 【O(log(n))time∣O(1)space】\\
Worst:\ 【O(n)time∣O(1)space】\\
```


```java
package NewArray;

/**
 * @author zhengstars
 * @date 2023/01/03
 */
public class BstConstruction {

    static class BST {
        public int value;
        public BST left;
        public BST right;

        public BST(int value) {
            this.value = value;
        }
    }

    
    public static BST inserValue(BST tree,int target){
        int currentNode = tree.value;

        while(true){
            //if target smaller than currentNode, search to the left
            if(target < currentNode){
                //if tree.left is null, then insert
                if(null == tree.left ){
                    tree.left = new BST(target);
                    break;
                }else{
                    tree = tree.left;
                }
            }else{
                //if target bigger than currentNode, search to the right
                //if tree.right is null, then insert
                if(null == tree.right ){
                    tree.right = new BST(target);
                    break;
                }else{
                    tree = tree.right;
                }
            }
        }

        return tree;
    }


    public static boolean containsValu(BST tree,int target){
        int currentNode = tree.value;

        // if tree is not null
        while(null != tree){
          if(target < currentNode){
              tree = tree.left;
          }else if(target > currentNode){
              tree = tree.right;
          }else{
              return true;
          }
        }
        return false;
    }



    public static BST deleteValue(BST tree,int target){
        BST parentNode = null;

        // tree is not null
        while(null != tree){
            // if target < tree.value
            if(target < tree.value){
                // save parent node value
                parentNode = tree;
                // Traverse the left subtree
                tree = tree.left;
            }
            // if target > tree.value
            else if(target > tree.value){
                // save parent node value
                parentNode = tree;
                // Traverse the right subtree
                tree = tree.right;
            }
            // if target == tree.value
            else{
                // if tree.left and tree.right are not null ,then a node has two child nodes
                if(null != tree.left && null != tree.right){
                    //Find the minimum value in the right subtree
                    tree.value = getMinValue(tree.right);

                    //Assign its value to the current node and delete the node where the minimum value is located
                    deleteValue(tree.right,tree.value);
                }
                // if parentNode is null
                // why is the special case, because the root node doesn't have a parent node
                // We should reassign one of the child nodes of the parent node
                else if(null == parentNode){
                    // if tree.left is not null, Reassign the left subtree
                    if(null != tree.left){
                        tree.value  = tree.left.value;
                        tree.right =  tree.left.right;
                        tree.left =  tree.left.left;
                    }
                    // if tree.right is not null, Reassign the right subtree
                    else if(null != tree.right){
                        tree.value  = tree.right.value;
                        tree.right =  tree.right.right;
                        tree.left =  tree.right.left;
                    }else{
                        //because null == parentNode
                        //The node does not have the left and right subtrees, which is equivalent to deleting the BST directly
                        tree = null;
                    }
                }

                // **parentNode.left == tree or parentNode.left == tree, the two cases are the case of the root node**
                // Only the left subtree or the right subtree

                // if parentNode.left == tree, then assigning the left child of the parent nodes.
                else if(parentNode.left == tree){
                    if(null != tree.left){
                        parentNode.left = tree.left;
                    }else{
                        parentNode.left = tree.right;
                    }
                }
                // if parentNode.left == tree, then assigning the right child of the parent nodes.
                else if(parentNode.right == tree){
                    if(null != tree.left){
                        parentNode.right = tree.left;
                    }else{
                        parentNode.right = tree.right;
                    }
                }

                // remember break at the end of all these if statements
                break;
            }
        }

        return tree;
    }


    /**
     * Get the minimum node value of BST
     * @param tree
     * @return
     */
    public static int getMinValue(BST tree){
        while(null != tree.left){
            tree = tree.left;
        }
        return tree.value;
    }

    public static void main(String[] args) {
        BST tree = new BST(10);
        

        BST tree1 = new BST(2);
        tree1.left = new BST(1);

        BST tree2 = new BST(5);
        tree2.left = tree1;
        tree2.right = new BST(6);

        BST tree3 = new BST(13);
        tree3.right = new BST(14);

        BST tree4 = new BST(15);
        tree4.right = new BST(22);
        tree4.left = tree3;

        tree.left = tree2;
        tree.right = tree4;

        BST root = deleteValue(tree,10);
        System.out.println(root);

    }

}

```

