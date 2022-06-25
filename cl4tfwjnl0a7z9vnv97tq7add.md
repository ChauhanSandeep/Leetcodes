## 665. Non-decreasing Array (25/06/2022)

## Problem:
Given an array  `nums`  with  `n`  integers, your task is to check if it could become non-decreasing by modifying  **at most one element**.

We define an array is non-decreasing if  `nums[i] <= nums[i + 1]`  holds for every  `i`  (**0-based**) such that (`0 <= i <= n - 2`).

**Example 1:**  <br>
**Input:** nums = [4,2,3] <br>
**Output:** true <br>
**Explanation:** You could modify the first `4` to `1` to get a non-decreasing array.

**Example 2:**  <br>
**Input:** nums = [4,2,1] <br>
**Output:** false <br>
**Explanation:** You can't get a non-decreasing array by modify at most one element.

**Constraints:**  <br>
-   `n == nums.length`
-   `1 <= n <= 104`
-   `-105  <= nums[i] <= 105`

## Solution:

**Algorithm**
if we find an element is less than its previous `(nums[i] < nums[i-1])` then there are two possibilities:
- If `(num[i-2] <= nums[i])` then `nums[i-1] = nums[i]`

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656134298449/j5ZidRyD6.png align="left")
- If `(nums[i-2] > nums[i])` then `nums[i] = nums[i-1]`

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656134321641/dSZfG2iZc.png align="left")

```Java
class Solution {
    /**
    nums[i] = next;
    nums[i-1] = curr;
    nums[i-2] = prev;
    
    if(prev <= next) curr = next
    if(prev > next) next = curr
    
    **/
    public boolean checkPossibility(int[] nums) {
        boolean modified = false; 
        
        for(int i = 1; i < nums.length; i++){
            if(nums[i] < nums[i-1]){
                if(modified) return false;
                modified = true;
                if(i == 1 || nums[i-2] <= nums[i]) nums[i-1] = nums[i];    
                else nums[i] = nums[i-1];
            }
        }
        return true;
    }
}
```
