## 406. Queue Reconstruction by Height (29/06/2022)

## Problem:

You are given an array of people,  `people`, which are the attributes of some people in a queue (not necessarily in order). Each  `people[i] = [hi, ki]`  represents the  `ith`  person of height  `hi`  with  **exactly**  `ki`  other people in front who have a height greater than or equal to  `hi`.

Reconstruct and return  _the queue that is represented by the input array_ `people`. The returned queue should be formatted as an array  `queue`, where  `queue[j] = [hj, kj]`  is the attributes of the  `jth`  person in the queue (`queue[0]`  is the person at the front of the queue).

**Example 1:** <br>
**Input:** people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]] <br>
**Output:** [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] <br>
**Explanation:**
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.

**Example 2:** <br>
**Input:** people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]] <br>
**Output:** [[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]

**Constraints:** <br>
-   `1 <= people.length <= 2000`
-   `0 <= hi  <= 106`
-   `0 <= ki  < people.length`
-   It is guaranteed that the queue can be reconstructed.

## Solution:

```Java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        int len = people.length;
        List<People> list = new ArrayList<>();
        for(int[] person: people) {
            list.add(new People(person[0], person[1]));
        }
       // Sort in ascending order of k if heights are same else sort in descending order  based on height
        Collections.sort(list, (a, b) -> (a.height == b.height) ? a.front - b.front : b.height - a.height);

        List<People> sortedList = new ArrayList<>();
        for (People person : list) {
           // insert at the index and shift the number if it's already present
           // like for (5,0) shift the (7,0) and then add 5 in 0 th position
            sortedList.add(person.front, person);
        }
        int[][] result = new int[len][2];
        for(int i=0; i<len; i++) {
            result[i] = sortedList.get(i).toArray();
        }
        return result;
    }
}

class People {
    int height;
    int front;

    public People(int height, int front) {
        this.height = height;
        this.front = front;
    }

    public int[] toArray() {
        return new int[] {height, front};
    }
}
```
**Time Complexity:**  `O(nlogn + n^2) = O(n^2)`: We sort the array in `O(nlogn)` time and list insertion will take `O(n^2)` for `n` insertions at worst.  <br>
**Space Complexity:**  `O(n)`