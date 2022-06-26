## 1642. Furthest Building You Can Reach (21/06/2022)

## Problem:

You are given an integer array  `heights`  representing the heights of buildings, some  `bricks`, and some  `ladders`.

You start your journey from building  `0`  and move to the next building by possibly using bricks or ladders.

While moving from building  `i`  to building  `i+1`  (**0-indexed**),

-   If the current building's height is  **greater than or equal**  to the next building's height, you do  **not**  need a ladder or bricks.
-   If the current building's height is  **less than**  the next building's height, you can either use  **one ladder**  or  `(h[i+1] - h[i])`  **bricks**.

_Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally._

**Example 1:** <br>
**Input:** heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1 <br>
**Output:** 4 <br>
**Explanation:** Starting at building 0, you can follow these steps:
- Go to building 1 without using ladders nor bricks since 4 >= 2.
- Go to building 2 using 5 bricks. You must use either bricks or ladders because 2 < 7.
- Go to building 3 without using ladders nor bricks since 7 >= 6.
- Go to building 4 using your only ladder. You must use either bricks or ladders because 6 < 9.
It is impossible to go beyond building 4 because you do not have any more bricks or ladders.

**Example 2:** <br>
**Input:** heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2 <br>
**Output:** 7

**Example 3:** <br>
**Input:** heights = [14,3,19,3], bricks = 17, ladders = 0 <br>
**Output:** 3

**Constraints:** <br>
-   `1 <= heights.length <= 105`
-   `1 <= heights[i] <= 106`
-   `0 <= bricks <= 109`
-   `0 <= ladders <= heights.length`

## Solution:

### Using Dynamic Programming (Time limit and memory limit exceeded)
**Algorithm**
- If the next building is smaller or equal to the current building then we can simply jump to next building without using bricks or a ladder.
- If the next building is bigger than the current building then we can use either bricks (equivalent to the difference between buildings) or one ladder.
- We need to follow this approach recursively and store maxReachable at each level.
- Keep an 3d matrix to store state at each level.
```Java
class Solution {
    private int maxReachable;
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        this.maxReachable = 0;
        boolean[][][] visited = new boolean[bricks+1][ladders+1][heights.length + 1];
        furthestBuilding(heights, bricks, ladders, 0, visited);
        return maxReachable;
    }
    
    private void furthestBuilding(int[] heights, int bricks, int ladders, int index, boolean[][][] visited) {
        maxReachable = Math.max(maxReachable, index);        
        if(index == heights.length - 1) return;
        if(visited[bricks][ladders][index]) return;
        
        int diff = heights[index+1] - heights[index];
        
        if(diff <= 0){
            furthestBuilding(heights, bricks, ladders, index+1, visited);
        }else{
            if(diff <= bricks) {
                furthestBuilding(heights, bricks - diff, ladders, index+1, visited);
            }
            if(ladders >= 1) {
                furthestBuilding(heights, bricks, ladders - 1, index+1, visited);
            }
        }
        visited[bricks][ladders][index] = true;
    }
}
```
**RCA:**
The above approach failed because of the constraints. We are exceeding the number of states we can store and the time it takes to calculate maxReachable for each state.
When DP approach fails, then we can try with the greedy algorithm.

### Using Greedy Approach
**Algorithm:**

> Always try to utilize bricks first. When bricks are finished, then we get the max jump taken till now. Try to use one ladder in that jump instead of bricks. 

- Iterate through the `heights` array while storing positive jumps(`diff`) in maxHeap and subtracting `diff` from `bricks`.
- If all bricks are utilized, get max jump from maxHeap and replace it with one ladder.
- If all ladders are also utilized then the current index is maximum reachable.

```Java
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        int len = heights.length;
        PriorityQueue<Integer> heap = new PriorityQueue<>((a, b) -> b - a);
        
        for(int i=0; i<len-1; i++) {
            int diff = heights[i+1] - heights[i];
            if(diff <= 0) continue;
            
            bricks = bricks - diff;
            heap.offer(diff);
            if(bricks < 0) {
                // swap last encountered max diff with one ladder
                if(ladders <= 0) return i;
                bricks += heap.poll();
                ladders--;
            }
        }
        return len-1;
    }
}
```