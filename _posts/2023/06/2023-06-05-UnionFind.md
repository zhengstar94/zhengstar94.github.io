---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Union Find"
date: "2023-06-05"
categories:
  - "Algorithm Famous"
---

# Union Find [Medium]

- Implement a simplified version of the Union-Find data structure in Java. The implementation should support the following operations:

  1. `createSet(value)`: Create a new set containing the given integer `value`.
  2. `find(value)`: Find the root node of the set to which the given integer belongs.`
  3. `createUnion(valueOne, valueTwo)`: Merge the sets containing `valueOne` and `valueTwo` if they do not already belong to the same set.



<br>

## Method 1

```tex
【O(1)time∣O(n)space】
```

```java
package Famous;

import java.util.HashMap;
import java.util.Map;

/**
*
* @author zhengstars
* @date 2023/11/18
*/
public class UnionFind {
    /**
     * Stores each element and its corresponding set.
     */
    private Map<Integer, Integer> disjointSet;

    /**
     * Stores the rank of each set.
     */
    private Map<Integer, Integer> rank;

    /**
     * Constructor to initialize the data structure.
     */
    public UnionFind() {
        // Initialize the hash map for storing sets.
        disjointSet = new HashMap<>();
        // Initialize the hash map for storing ranks.
        rank = new HashMap<>();
    }

    /**
     * Creates a new set containing the given integer value.
     *
     * @param value The integer to be added to the set.
     */
    public void createSet(int value) {
        // Add the element to the set, initially each element forms its own set.
        disjointSet.put(value, value);
        // The initial rank of each set is 0.
        rank.put(value, 0);
    }

    /**
     * Finds the root node of the set to which the given integer belongs.
     *
     * @param value The integer to be found in the set.
     * @return The root node of the set to which the integer belongs.
     */
    public int find(int value) {
        if (!disjointSet.containsKey(value)) {
            // If the value does not exist, you can choose to return a special marker value, such as -1,
            // or throw an exception, depending on the circumstances.
            return -1;
        }
        if (disjointSet.get(value) != value) {
            // Path compression: directly connect the element to the root node.
            disjointSet.put(value, find(disjointSet.get(value)));
        }
        return disjointSet.get(value);
    }

    /**
     * Merges the two sets containing valueOne and valueTwo.
     *
     * @param valueOne The first integer.
     * @param valueTwo The second integer.
     */
    public void createUnion(int valueOne, int valueTwo) {
        if (!disjointSet.containsKey(valueOne) || !disjointSet.containsKey(valueTwo)) {
            // If either value is not present in the set, do not perform the union operation.
            return;
        }
        // Find the root node of the set to which the first element belongs.
        int rootOne = find(valueOne);
        // Find the root node of the set to which the second element belongs.
        int rootTwo = find(valueTwo);
        if (rootOne == rootTwo) {
            // If both elements already belong to the same set, do not perform the union operation.
            return;
        } else {
            if (rank.get(rootOne) < rank.get(rootTwo)) {
                // Connect the root node of the first set to the root node of the second set.
                disjointSet.put(rootOne, rootTwo);
            } else if (rank.get(rootOne) > rank.get(rootTwo)) {
                // Connect the root node of the second set to the root node of the first set.
                disjointSet.put(rootTwo, rootOne);
            } else {
                // Connect the root node of the second set to the root node of the first set.
                disjointSet.put(rootTwo, rootOne);
                // Update the rank of the root node.
                rank.put(rootOne, rank.get(rootOne) + 1);
            }
        }
    }

    public static void main(String[] args) {
        // Create an instance of the UnionFind class.
        UnionFind uf = new UnionFind();

        // Create sets and test the find operation.
        uf.createSet(1);
        uf.createSet(2);
        uf.createSet(3);

        // Should output 1
        System.out.println("Find(1): " + uf.find(1));
        // Should output 3
        System.out.println("Find(3): " + uf.find(3));

        // Test the createUnion operation.
        uf.createUnion(1, 2);
        uf.createUnion(2, 3);

        // Should output 1 because 1 and 2 are merged.
        System.out.println("Find(1): " + uf.find(1));
        // Should output 1 because 3 and 2 are merged.
        System.out.println("Find(3): " + uf.find(3));

        // Another set of tests.
        uf.createSet(4);
        uf.createSet(5);
        uf.createSet(6);

        uf.createUnion(4, 5);
        uf.createUnion(5, 6);

        // Should output 4
        System.out.println("Find(4): " + uf.find(4));
        // Should output 4 because 5 and 6 are merged.
        System.out.println("Find(6): " + uf.find(6));
    }
}

```



## Method 2 (Recommend)

```tex
【O(1)time∣O(n)space】
```

```java
package Famous;

import java.util.HashMap;
import java.util.Map;

/**
 * UnionFind3 class represents a Union-Find data structure with path compression
 * and union by rank optimization.
 *
 * @author zhengstars
 * @date 2023/11/19
 */
public class UnionFind3 {
  /**
   * Array to store the parent (root) of each element in the disjoint sets.
   */
  int[] root;

  /**
   * Array to store the rank of each disjoint set.
   */
  int[] rank;

  /**
   * Constructs a UnionFind3 instance with a given number of elements.
   *
   * @param n The number of elements for the disjoint sets.
   */
  public UnionFind3(int n) {
    // Initialize the root and rank arrays.
    root = new int[n];
    rank = new int[n];

    for (int i = 0; i < n; i++) {
      // Initially, each element is its own root, and rank is 0.
      root[i] = i;
      rank[i] = 0;
    }
  }

  /**
   * Finds the root (representative) of the disjoint set to which x belongs,
   * with path compression to optimize future find operations.
   *
   * @param x The element to find.
   * @return The root of the disjoint set to which x belongs.
   */
  public int find(int x) {
    // If x is not the root, update its parent to the root recursively.
    if (x == root[x]) {
      return root[x];
    }

    // Path compression: Set the parent of x to the root of its set.
    return root[x] = find(root[x]);
  }

  /**
   * Unites the disjoint sets to which x and y belong using union by rank.
   *
   * @param x The representative element of the first set.
   * @param y The representative element of the second set.
   */
  public void union(int x, int y) {
    // Find the roots of the sets to which x and y belong.
    int rootX = find(x);
    int rootY = find(y);

    // If the roots are different, perform union based on rank.
    if (rootX != rootY) {
      if (rank[rootX] > rank[rootY]) {
        // Attach the set with lower rank to the one with higher rank.
        root[rootY] = rootX;
      } else if (rank[rootX] < rank[rootY]) {
        // Attach the set with lower rank to the one with higher rank.
        root[rootX] = rootY;
      } else {
        // If ranks are equal, attach one set to the other and increment the rank.
        root[rootY] = rootX;
        rank[rootX] += 1;
      }
    }
  }

  /**
   * Main method to demonstrate the usage of UnionFind3.
   *
   * @param args Command line arguments (not used).
   */
  public static void main(String[] args) {
    // Create an instance of UnionFind3 with 5 elements.
    UnionFind3 uf = new UnionFind3(5);

    // Demonstrate find operation before any unions.
    System.out.println("Find(1): " + uf.find(1));
    System.out.println("Find(3): " + uf.find(3));

    // Perform unions to merge sets.
    uf.union(0, 1);
    uf.union(0, 2);
    uf.union(3, 4);

    // Demonstrate find operation after unions.
    System.out.println("Find(0): " + uf.find(0));
    System.out.println("Find(1): " + uf.find(1));
    System.out.println("Find(2): " + uf.find(2));
    System.out.println("Find(3): " + uf.find(3));
    System.out.println("Find(4): " + uf.find(4));

    // Further union to demonstrate the effect of union by rank.
    uf.union(2, 4);

    // Demonstrate find operation after additional union.
    System.out.println("=================");
    System.out.println("Find(0): " + uf.find(0));
    System.out.println("Find(1): " + uf.find(1));
    System.out.println("Find(2): " + uf.find(2));
    System.out.println("Find(3): " + uf.find(3));
    System.out.println("Find(4): " + uf.find(4));
  }
}
```

