# Merge Overlapping Intervals [Medium]

- Write a function that takes in a non-empty array of arbitrary intervals, merges any overlapping intervals, and returns the new intervals in no particular order.
- Each interval `interval `is an array of two integers,with `interval[0]`as the start of the interval and `interval[1]` as the end of the interval.
- Note that back-to-back intervals aren't considered to be overlapping. For example, `[1,5]` and `[6,7]`aren't overlapping; however,  `[1,6]`and`[6,7]`are indeed overlapping.
- Also note that the start of any particular interval will always be less than or equal to the end of that interval.

**Sample Input #1**

> intervals=[[1,2],[3,5],[4,7],[6,8],[9,10]]

**Sample Output #1**

> [[1,2],[3,8],[9,10]]
>
> // Merge the intervals [3,5],[4,7],and [6,8].
>
> // The intervals could be ordered differently.

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> The problem asks you to merge overlapping intervals.How can you determine if two intervals are overlapping?</strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Sort the intervals with respect to their starting values.This will allow you to merge all overlapping intervals in a single traversal through the sorted intervals.  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> After sorting the intervals with respect to their starting values,traverse them,and at each iteration,compare the start of the next interval to the end of the current interval to look for an overlap.If you find an overlap,mutate the current interval so as to merge the next interval into it.  </strong></i>
</details>

<br>

## Method 1

```tex
【 O(nlog(n)) time | O(n) space 】
```



```java
import java.util.*;

class Program {

  public int[][] mergeOverlappingIntervals(int[][] intervals) {
     if (intervals.length <= 1) {
            return intervals;
        }

        //Array sorting
        Arrays.sort(intervals, (a, b) -> (a[0] - b[0]));

        List<int[]> result = new ArrayList<>();
        for (int[] interval : intervals) {
            // if list is empty or does not overlap with the previous, just append
            if (result.isEmpty() || result.get(result.size() - 1)[1] < interval[0]) {
                result.add(interval);
            } else {
                // if overlap, merge the current and previous intervals
                result.get(result.size() - 1)[1] = Math.max(result.get(result.size() - 1)[1], interval[1]);
            }
        }

        return result.toArray(new int[result.size()][]);
  }
}
```


