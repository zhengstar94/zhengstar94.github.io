---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1352. Product of the Last K Numbers"
date: "2025-02-14"
tags: Medium
categories:
  - "LeetCode Math"
---


- Design an algorithm that accepts a stream of integers and retrieves (vt. 重新得到；恢复；检索 vi. 找回猎物) the product of the last `k` integers of the stream.
- Implement the `ProductOfNumbers` class:
  - `ProductOfNumbers()` Initializes the object with an empty stream.
  - `void add(int num)` Appends the integer `num` to the stream.
  - `int getProduct(int k)` Returns the product of the last `k` numbers in the current list. You can assume that always the current list has at least `k` numbers.
- The test cases are generated so that, at any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.


**Example 1**

```
Input
["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
[[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

Output
[null,null,null,null,null,null,20,40,0,null,32]

Explanation
ProductOfNumbers productOfNumbers = new ProductOfNumbers();
productOfNumbers.add(3);        // [3]
productOfNumbers.add(0);        // [3,0]
productOfNumbers.add(2);        // [3,0,2]
productOfNumbers.add(5);        // [3,0,2,5]
productOfNumbers.add(4);        // [3,0,2,5,4]
productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
productOfNumbers.add(8);        // [3,0,2,5,4,8]
productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32 
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

import java.util.ArrayList;
import java.util.List;

/**
 * Author: zhengxingxing
 * Date: 2025/02/14
 */
public class ProductOfNumbers {
    // Use ArrayList to store the prefix product of the numbers in the stream.
    // prefixProduct[i] represents the product of all numbers from the start of the list to index i.
    private static List<Integer> prefixProduct;
    
    public ProductOfNumbers() {
        prefixProduct = new ArrayList<>();
        prefixProduct.add(1); // Add 1 as the initial base of the prefix product list.
    }
    
    public void add(int num) {
        if (num == 0) {
            // If the number is 0, reset the prefix product list.
            // This resets the product calculation due to the zero.
            prefixProduct = new ArrayList<>();
            prefixProduct.add(1); // Start fresh with 1 again as the base.
        } else {
            // If the number is not zero, calculate the new prefix product 
            // by multiplying the last prefix product with the new number.
            prefixProduct.add(prefixProduct.get(prefixProduct.size() - 1) * num);
        }
    }
    
    public int getProduct(int k) {
        int n = prefixProduct.size();
        // If `k` is greater than or equal to the size of the prefix list,
        // it means the range includes a zero, so the product is 0.
        if (k >= n) {
            return 0;
        }
        // Return the product of the last `k` numbers using:
        // prefixProduct[n-1] (total product up to the last number)
        // divided by prefixProduct[n-k-1] (product up to the last `n-k` numbers).
        return prefixProduct.get(n - 1) / prefixProduct.get(n - k - 1);
    }
    
    public static void main(String[] args) {
        ProductOfNumbers productOfNumbers = new ProductOfNumbers();

        // Test case 1
        productOfNumbers.add(3); // Stream: [3]
        productOfNumbers.add(0); // Stream: [3,0] - reset due to zero
        productOfNumbers.add(2); // Stream: [2] - starting fresh
        productOfNumbers.add(5); // Stream: [2,5]
        productOfNumbers.add(4); // Stream: [2,5,4]
        System.out.println(productOfNumbers.getProduct(2)); // Expected output: 20 (5 * 4)
        System.out.println(productOfNumbers.getProduct(3)); // Expected output: 40 (2 * 5 * 4)
        System.out.println(productOfNumbers.getProduct(4)); // Expected output: 0 (due to zero in the range)
        productOfNumbers.add(8); // Stream: [2,5,4,8]
        System.out.println(productOfNumbers.getProduct(2)); // Expected output: 32 (4 * 8)

        // Test case 2
        ProductOfNumbers test2 = new ProductOfNumbers();
        test2.add(2); // Stream: [2]
        test2.add(3); // Stream: [2,3]
        test2.add(4); // Stream: [2,3,4]
        System.out.println(test2.getProduct(2)); // Expected output: 12 (3 * 4)
        System.out.println(test2.getProduct(3)); // Expected output: 24 (2 * 3 * 4)
    }
}

```





