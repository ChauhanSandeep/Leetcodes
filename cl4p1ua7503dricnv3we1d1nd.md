## 215. Kth Largest Element in an Array (22/06/2022)

### Problem:

Given an integer array  `nums`  and an integer  `k`, return  _the_  `kth`  _largest element in the array_.

Note that it is the  `kth`  largest element in the sorted order, not the  `kth`  distinct element.

**Example 1:** <br>
**Input:** nums = [3,2,1,5,6,4], k = 2 <br>
**Output:** 5

**Example 2:** <br>
**Input:** nums = [3,2,3,1,2,4,5,5,6], k = 4 <br>
**Output:** 4

**Constraints:** <br>
-   `1 <= k <= nums.length <= 104`
-   `-104  <= nums[i] <= 104`

### Solution:
#### Using sorting
**Algorithm**
- Sort `nums` array.
- Element present at index `nums.length - k` would be `kth` largest.

```Java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }
}
```
**Time complexity:** `O(n log n)` where n is size of `nums` array

**Space complexity:** `O(1)` as we are not using any extra data structure

#### Using Heap
**Algorithm**
> In a min-heap, top element is smallest among all the elements present.

- Iterate through `nums` array and place each element `num` in a min heap (`heap`).
- If `heap` size is greater than `k` then pop element from `heap`
- After iteration is completed, top element in `heap` would be `kth` largest
```Java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        
        for(int num: nums) {
            heap.offer(num);
            if(heap.size() > k) {
                heap.poll();
            }
        }
        return heap.peek();
    }
}
```
**Time complexity:** `O(n log k)` where n is size of `nums` array

**Space complexity:** `O(k)` as we are using min-heap to store k elements