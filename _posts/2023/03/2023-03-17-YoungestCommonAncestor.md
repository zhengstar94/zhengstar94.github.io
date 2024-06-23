---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Youngest Common Ancestor"
date: "2023-03-17"
categories:
  - "Algorithm Graphs"
---

# Youngest Common Ancestor [Medium]

- You're given three inputs,all of which are instances of an `AncestralTree` class that have an `ancestor` property pointing to their youngest ancestor. The first input is the top ancestor in an ancestral tree (i.e., the only instance that has no ancestor--its `ancestor` property points to `None`/`null` ), and the other two inputs are descendants in the ancestral tree.

- Write a function that returns the youngest common ancestor to the two descendants.

- Note that a descendant is considered its own ancestor. So in the simple ancestral tree below, the youngest common ancestor to nodes A and B is node A.



````
// The youngest common ancestor to nodes A and B is node A.
    A
   /
  B 
````



**Sample Input **

```
// The nodes are from the ancestral tree below.
topAncestor = node A 
descendantone = node E 
descendantTwo = node I

                      A
                   /     \
                 B         C
                / \       /  \
               D    E    F    G
              / \     
             H   I     
```

**Sample Output **

```
node B
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Since you must return the sizes of rivers,which consist of horizontally and vertically adjacent 1s in the input matrix,you must somehow keep track of groups of neighboring 1s as you traverse the matrix. Try treating the matrix as a graph, where each element in the matrix is a node in the graph with up to 4 neighboring nodes (above, below, to the left, and to the right), and traverse it using a popular graph-traversal algorithm like Depth-first Search or Breadth-first Search. </strong></i>
</details>


<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> By traversing the matrix using DFS or BFS as mentioned in Hint #1, any time that you encounter a 1 you can traverse the entire river that this 1 is a part of (and keep track of its size)by simply iterating through the given node's neighboring nodes and their own neighboring nodes so long as the nodes are 1s. </strong></i>
</details>


<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Naturally, many nodes in the graph mentioned in Hint #1 will have overlapping neighboring nodes, and as you traverse the matrix, you will undoubtedly encounter nodes that you have previously visited.In order to prevent mistakenly calculating the same river's size multiple times and to avoid doing needless computational work, try keeping track of every node that you visit in an auxiliary data structure and only performing important computations on unvisited nodes. What data structure would be ideal here? </strong></i>
</details>


<br>

## Method 1

```tex
【O(h)time∣O(1)space】
```

```java
package Graphs;

/**
 * @author zhengstars
 * @date 2023/03/11
 */
public class YoungestCommonAncestor {
    static class AncestralTree {
        public char name;
        public AncestralTree ancestor;

        AncestralTree(char name) {
            this.name = name;
            this.ancestor = null;
        }

        // This method is for testing only.
        void addAsAncestor(AncestralTree[] descendants) {
            for (AncestralTree descendant : descendants) {
                descendant.ancestor = this;
            }
        }
    }

    public static AncestralTree getYoungestCommonAncestor(
            AncestralTree topAncestor, AncestralTree descendantOne, AncestralTree descendantTwo) {
        int depthOne = getDescendantDepth(descendantOne,topAncestor);
        int depthTwo = getDescendantDepth(descendantTwo,topAncestor);
        if(depthOne > depthTwo){
            return backtrackAncestralTree(descendantOne, descendantTwo, depthOne - depthTwo);
        }else{
            return backtrackAncestralTree(descendantTwo, descendantOne, depthTwo - depthOne);
        }

    }

    // Calculates the depth of the descendant relative to the top ancestor.
    private static int getDescendantDepth(AncestralTree descendant, AncestralTree topAncestor) {
        int depth = 0;

        while(descendant != topAncestor){
            depth++;
            descendant = descendant.ancestor;
        }
        return depth;
    }

    // Backtracks from the deeper descendant until both descendants are at the same depth.
    public static AncestralTree backtrackAncestralTree(AncestralTree lowerDescendant, AncestralTree higherDescendant, int diff) {
        while (diff > 0) {
            lowerDescendant = lowerDescendant.ancestor;
            diff--;
        }
        while (lowerDescendant != higherDescendant) {
            lowerDescendant = lowerDescendant.ancestor;
            higherDescendant = higherDescendant.ancestor;
        }
        return lowerDescendant;
    }

    public static void main(String[] args) {
        AncestralTree A = new AncestralTree('A');
        AncestralTree B = new AncestralTree('B');
        AncestralTree C = new AncestralTree('C');
        AncestralTree D = new AncestralTree('D');
        AncestralTree E = new AncestralTree('E');
        AncestralTree F = new AncestralTree('F');
        AncestralTree G = new AncestralTree('G');
        AncestralTree H = new AncestralTree('H');
        AncestralTree I = new AncestralTree('I');

        A.addAsAncestor(new AncestralTree[] { B, C });
        B.addAsAncestor(new AncestralTree[] { D, E });
        C.addAsAncestor(new AncestralTree[] { F, G });
        D.addAsAncestor(new AncestralTree[] { H, I });

        AncestralTree youngestCommonAncestor =
                getYoungestCommonAncestor(A, E, I);
        // Output: B
        System.out.println(youngestCommonAncestor.name); 
    }
}
```
