---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "347.Top K Frequent Elements"
date: "2024-02-06"
categories:
  - "LeetCode Array"
---

# LeetCode 347. Top K Frequent Elements [Easy]

- Given an integer array `nums` and an integer `k`, return *the* `k` *most frequent elements*. You may return the answer in **any order**.

**Example 1**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2**

```
Input: nums = [1], k = 1
Output: [1]
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;

/**
 * @author zhengstars
 * @date 2024/03/03
 */
public class TopKFrequentElements {
    public static int[] topKFrequent(int[] nums, int k) {
        // Declare a HashMap to hold the frequency of each number
        HashMap<Integer,Integer> map = new HashMap<>();

        // Loop over the array and update the frequency in the HashMap
        for (int num : nums){
            if(map.containsKey(num)){
                // If the number is already in the map, increment its frequency by 1
                map.put(num,map.get(num)+1);
            }else{
                // Otherwise, set the frequency of the number to 1
                map.put(num,1);
            }
        }

        // Create an array of lists where the index is the frequency and each list holds all numbers of that frequency
        List<Integer>[] list = new List[nums.length + 1];
        for (int key : map.keySet()){
            int i = map.get(key); // Get the frequency of the number
            if(list[i] == null){
                // If there isn't a list for this frequency yet, create one
                list[i] = new ArrayList<>();
            }
            // Add the number to the list of its frequency
            list[i].add(key);
        }

        // Create an array to store the result
        int[] result = new int[k];
        int n = 0; // Initialize index n
        // Iterate over the array of lists from highest frequency to lowest
        for (int i = list.length - 1; i >= 0 && n < k; i--) {
            /* We iterate from the end of the list to the beginning and also confirm if we have found less than k elements */

            if(list[i] == null){
                continue;
            }

            for (int j = 0; j < list[i].size() && n < k; j++){
                /* We iterate over the elements corresponding to frequency i and also confirm if we have found less than k elements */
                /* We add each element corresponding to frequency i to the result array and check if we have already found k elements to avoid unnecessary operations */
                result[n++] = list[i].get(j);
                }
        }

        // Return the result array
        return result;
    }

    public static void main(String[] args) {
        int[] nums1 = {1, 1, 1, 2, 2, 3};
        int k1 = 2;
        // Print the result
        System.out.println(Arrays.toString(topKFrequent(nums1, k1)));  // Expected output: [1,2]

        int[] nums2 = {1};
        int k2 = 1;
        // Print the result
        System.out.println(Arrays.toString(topKFrequent(nums2, k2)));  // Expected output: [1]
    }
}
```

