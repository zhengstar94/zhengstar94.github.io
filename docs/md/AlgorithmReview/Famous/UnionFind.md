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



## Method 2

```tex
【O(n^2)time∣O(n)space】
```

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

public class StableInternships {
    public static int[][] findStableMatches(int[][] interns, int[][] companies) {
        int n = interns.length; // 获取实习生的数量
        int[] internMatch = new int[n]; // 存储实习生的匹配结果
        int[] companyMatch = new int[n]; // 存储公司的匹配结果
        boolean[] internFree = new boolean[n]; // 标记实习生是否已被匹配
        Queue<Integer> freeInterns = new LinkedList<>(); // 存储尚未匹配的实习生

        // 初始化匹配状态
        Arrays.fill(internMatch, -1);
        Arrays.fill(companyMatch, -1);
        Arrays.fill(internFree, true);
        for (int i = 0; i < n; i++) {
            freeInterns.add(i); // 最初所有实习生都是自由的
        }

        // 当还有自由的实习生时，继续进行匹配
        while (!freeInterns.isEmpty()) {
            int intern = freeInterns.poll(); // 取出一个自由的实习生
            for (int i = 0; i < n && internFree[intern]; i++) {
                int company = interns[intern][i]; // 实习生偏好列表中的公司
                if (companyMatch[company] == -1) {
                    // 如果公司还未匹配，进行匹配
                    companyMatch[company] = intern;
                    internMatch[intern] = company;
                    internFree[intern] = false;
                } else {
                    // 如果公司已有匹配，检查是否更愿意匹配当前实习生
                    int currentIntern = companyMatch[company];
                    if (prefers(companies[company], intern, currentIntern)) {
                        // 如果公司更偏好当前实习生，进行重新匹配
                        companyMatch[company] = intern;
                        internMatch[intern] = company;
                        internFree[intern] = false;
                        internFree[currentIntern] = true; // 之前的实习生变为自由状态
                        freeInterns.add(currentIntern); // 将之前的实习生加回队列
                    }
                }
            }
        }

        // 构建最终的匹配结果数组
        int[][] pairs = new int[n][2];
        for (int i = 0; i < n; i++) {
            pairs[i][0] = i; // 实习生索引
            pairs[i][1] = internMatch[i]; // 匹配到的公司索引
        }
        return pairs;
    }

    // 检查公司是否更偏好新的实习生
    private static boolean prefers(int[] companyPreferences, int intern, int currentIntern) {
        for (int preference : companyPreferences) {
            if (preference == intern) {
                return true; // 更偏好新的实习生
            }
            if (preference == currentIntern) {
                return false; // 更偏好当前的实习生
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[][] interns = {{0, 1, 2}, {1, 0, 2}, {2, 1, 0}};
        int[][] companies = {{2, 0, 1}, {0, 2, 1}, {1, 2, 0}};
        int[][] matches = findStableMatches(interns, companies);
        for (int[] match : matches) {
            System.out.println("实习生 " + match[0] + " 匹配到公司 " + match[1]);
        }
    }
}

```

