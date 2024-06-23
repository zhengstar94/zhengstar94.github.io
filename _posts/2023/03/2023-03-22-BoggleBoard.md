---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Boggle Board"
date: "2023-03-22"
categories:
  - "Algorithm Graphs"
---

# Boggle Board [Hard]

- You're given a two-dimensional array (a matrix)of potentially unequal height and width containing letters; this matrix represents a boggle board. You're also given a list of words.
- Write a function that returns an array of all the words contained in the boggle board. The final words don't need to be in any particular order.
- A word is constructed in the boggle board by connecting adjacent (horizontally, vertically, or diagonally) letters, without using any single letter at a given position more than once; while a word can of course have repeated letters, those repeated letters must come from different positions in the boggle board in order for the word to be contained in the board. Note that two or more words are allowed to overlap and use the same letters in the boggle board

**Sample Input**

```
board = [
 	["t","h","i","s","i","s","a"],
 	["s","i","m","p","l","e","x"],
 	["b","x","x","x","x","e","b"],
 	["x","o","g","g","1","x","o"],
 	["x","x","x","D","T","r","a"],
 	["R","E","P","E","A","d","x"],
 	["x","x","x","x","x","x","x"],
 	["N","O","T","R","E","-","P"],
 	["x","x","D","E","T","A","E"],
 ],
words = [
	"this","is","not","a""simple""boggle",
	"board","test","REPEATED","NOTRE-PEATED",
]
```

**Sample Output**

```
["this","is","a","simple","boggle""board","NOTRE-PEATED"]
// The words could be ordered differently.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> You can divide this question into two separate problems: one part involves traversing the boggle board in such a way that allows you to construct strings letter by letter; the other part involves actually comparing the strings you construct in the board against the words in the list that you're given.For the second part, what data structure lends itself very well to matching characters to multiple strings at once? </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Try creating a trie out of the input list of words. This will allow you to compare letters in the boggle board against all input words in constant time. How can you efficiently traverse the boggle board to construct all potentially valid strings,without counting letters twice in any string? </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Treat the board as a graph, where each element in the board is a node with up to 8 neighboring nodes. Traverse it in a depth-first-search-like fashion, checking if letters are contained in the trie and traversing the trie simultaneously if it makes sense to do so. How can you keep track of letters that you've already visited in order to avoid erroneously counting some of them twice in a single string? Could you keep track of visited nodes in an auxiliary data structure? </strong></i>
</details>




<br>



<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> Keeping in mind that you only want to mark nodes as visited in a single branch of the graph that you're traversing (i.e., you don't want the state of visited nodes in one branch of the graph to spill into the state of another branch of the graph), try marking any node you traverse as unvisited at the end of the recursive call that actually traverses it, after
traversing through all of the node's neighbors and performing the same actions on them recursively.</strong></i>
</details>


<br>



## Method 1

```tex
【O(NM8^S+L)time∣O(SL + MN)space】
```

```tex
1.1 N\ and\ M\ are\ the\ rows\ and\ columns\ of\ a\ two-dimensional\ character\ array,\ S\ is\ the\ average\ length\ of\ words\ to\ find,\ and\ L\ is\ the\ number\ of\ words\ to\ find.\ 8^S\ represents\ the\ possibility\ of\ adjacent\ positions\\

2.1 \ S\ is\ the\ number\ of\ words\ ,\ L\ is\ the\ average\ length\ of\ words\\
2.2 \ The\ space\ complexity\ of\ dictionary\ tree\ is\ O(SL)\\
2.3 \ The\ space\ occupied\ by\ visited\ array\ is\ O(MN)\\
```



```java
package Graphs;

import java.util.*;

class BoggleBoard {
    public static List<String> boggleBoard(char[][] board, String[] words) {
        // Create a Trie data structure to store the words to be searched
        Trie trie = new Trie();
        for (String word : words) {
            trie.addWord(word);
        }

        Set<String> foundWords = new HashSet<>();
        boolean[][] visited = new boolean[board.length][board[0].length];
        // Execute depth-first search for each position on the board
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                search(i, j, board, trie.root, visited, foundWords);
            }
        }
        return new ArrayList<String>(foundWords);
    }

    // Recursive function for depth-first search
    private static void search(int i, int j, char[][] board, TrieNode node, boolean[][] visited, Set<String> foundWords) {
        // Check if the current position has already been visited
        if (visited[i][j]) {
            return;
        }
        char letter = board[i][j];
        // If the prefix of the word does not exist in the Trie, return directly
        if (!node.children.containsKey(letter)) {
            return;
        }
        // Mark the current position as visited
        visited[i][j] = true;
        // Move the current node to the next position
        node = node.children.get(letter);
        // If the current node is the end of a word, add the word to the result set
        if (node.isWord) {
            foundWords.add(node.word);
        }
        // Recursively visit the adjacent 8 positions
        for (int row = i - 1; row <= i + 1 && row < board.length; row++) {
            for (int col = j - 1; col <= j + 1 && col < board[0].length; col++) {
                if (row >= 0 && col >= 0) {
                    search(row, col, board, node, visited, foundWords);
                }
            }
        }
        // Mark the current position as unvisited
        visited[i][j] = false;
    }
  
	public static void main(String[] args) {
        char[][] board = {
                {'t', 'h', 'i', 's', 'i', 's', 'a'},
                {'s', 'i', 'm', 'p', 'l', 'e', 'x'},
                {'b', 'x', 'x', 'x', 'x', 'e', 'b'},
                {'x', 'o', 'g', 'g', '1', 'x', 'o'},
                {'x', 'x', 'x', 'D', 'T', 'r', 'a'},
                {'R', 'E', 'P', 'E', 'A', 'd', 'x'},
                {'x', 'x', 'x', 'x', 'x', 'x', 'x'},
                {'N', 'O', 'T', 'R', 'E', '-', 'P'},
                {'x', 'x', 'D', 'E', 'T', 'A', 'E'},
        };
        String[] words = {"thisisa","tsbxxRxNx","this", "is", "not", "a", "simple", "boggle", "board", "test", "REPEATED", "NOTRE-PEATED"};
        List<String> result = boggleBoard(board, words);
        System.out.println(result);

    }
}

/**
 * Trie tree node
 */
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isWord = false;
    String word = "";
}

/**
 * Trie tree
 */
class Trie {
    TrieNode root = new TrieNode();

    // Add a word to the Trie tree
    public void addWord(String word) {
        TrieNode node = root;
        for (char letter : word.toCharArray()) {
            if (!node.children.containsKey(letter)) {
                node.children.put(letter, new TrieNode());
            }
            node = node.children.get(letter);
        }
        node.isWord = true;
        node.word = word;
    }
}

```



## Method 2

```tex
【O(MNL + 8MN)time∣O(SL + MN)space】
```

```tex
1.1 \ The\ average\ length\ of\ all\ words\ in\ the\ dictionary\ is\ L\ and\ the\ matrix\ size\ is\ M*N\\
1.2 \ O(MNL)\ is\ the\ time\ complexity\ of\ building\ a\ dictionary\ tree\\
1.3 \ O(8MN)\ is\ the\ time\ complexity\ of\ DFS\ each\ lattice\\

2.1 \ S\ is\ the\ number\ of\ words\ ,\ L\ is\ the\ average\ length\ of\ words\\
2.2 \ The\ space\ complexity\ of\ dictionary\ tree\ is\ O(SL)\\
2.3 \ The\ space\ occupied\ by\ visited\ array\ is\ O(MN)\\
```



```java
package Graphs;

import com.sun.deploy.util.StringUtils;

import java.util.*;

/**
 * @author zhengstars
 * @date 2023/03/24
 */

/**
 * Main idea：
 *
 * 1. Build a trie to store all the words in the dictionary.
 *
 * 2. For each grid, search for words starting with the character of this grid on the trie,
 * use DFS to search the adjacent grids in the eight directions of up, down, left, right, and diagonals,
 * recursively search until reaching a node that does not exist in the trie, and return to the previous grid to continue the search.
 *
 *
 * - Time complexity: Assuming the average length of all words in the dictionary is L and the size of the matrix is M x N,
 * the time complexity is O(MNL + 8MN), where O(MNL) is the time complexity for building the trie,
 * and O(8MN) is the time complexity for performing DFS for each grid.
 *
 * - Space complexity: The space complexity of the trie is O(SL), where S is the number of words and L is the average length of words.
 * The space occupied by visited array is O (MN). Therefore, the total space complexity is O (SL + MN).
 */

public class BoggleBoard3 {
    public static List<String> boggleBoard(char[][] board, String[] words) {
        // Build a trie data structure to store the given words
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode node = root;
            // Traverse the word and add each character to the trie
            for (char c : word.toCharArray()) {
                if (!node.children.containsKey(c)) {
                    node.children.put(c, new TrieNode());
                }
                node = node.children.get(c);
            }
            // Mark the last node as a word node
            node.word = word;
        }

        // Use DFS to search for valid words on the board
        Set<String> result = new HashSet<String>();
        int row = board.length;
        int col = board[0].length;
        boolean[][] visited = new boolean[row][col];

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                dfs(board, visited, i, j, root, result);
            }
        }
        // Convert the result set to a list and return it
        return new ArrayList<String>(result);
    }

    private static void dfs(char[][] board, boolean[][] visited, int i, int j, TrieNode node, Set<String> result) {
        char c = board[i][j];
        // Check if the current character exists in the trie
        if (!node.children.containsKey(c)) {
            return;
        }

        // Mark the current cell as visited and move to the next node in the trie
        visited[i][j] = true;
        node = node.children.get(c);

        // Check if the current node is a word node and add it to the result set
        if (!node.word.isEmpty()) {
            result.add(node.word);
        }

        // Define the possible directions for the next move on the board
        int[][] directions = { {1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, 1}, {-1, -1}, {-1, 1}, {1, -1}};

        // Recursively search for valid words in each direction
        for (int[] dir : directions) {
            int x = i + dir[0];
            int y = j + dir[1];

            // Check if the next cell is out of bounds or already visited
            if (x < 0 || x >= board.length || y < 0 || y >= board[0].length || visited[x][y]) {
                continue;
            }
            dfs(board, visited, x, y, node, result);
        }

        // Mark the current cell as unvisited before backtracking
        visited[i][j] = false;
    }

    public static void main(String[] args) {
        char[][] board = {
                {'t', 'h', 'i', 's', 'i', 's', 'a'},
                {'s', 'i', 'm', 'p', 'l', 'e', 'x'},
                {'b', 'x', 'x', 'x', 'x', 'e', 'b'},
                {'x', 'o', 'g', 'g', '1', 'x', 'o'},
                {'x', 'x', 'x', 'D', 'T', 'r', 'a'},
                {'R', 'E', 'P', 'E', 'A', 'd', 'x'},
                {'x', 'x', 'x', 'x', 'x', 'x', 'x'},
                {'N', 'O', 'T', 'R', 'E', '-', 'P'},
                {'x', 'x', 'D', 'E', 'T', 'A', 'E'},
        };
        String[] words = {"thisisa","tsbxxRxNx","this", "is", "not", "a", "simple", "boggle", "board", "test", "REPEATED", "NOTRE-PEATED"};
        List<String> result = boggleBoard(board, words);
        System.out.println(result);

    }
}

```


