## 1423. Maximum Points You Can Obtain from Cards (26/06/2022)

## Problem:

There are several cards  **arranged in a row**, and each card has an associated number of points. The points are given in the integer array  `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly  `k`  cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array  `cardPoints`  and the integer  `k`, return the  _maximum score_  you can obtain.

**Example 1:** <br>
**Input:** cardPoints = [1,2,3,4,5,6,1], k = 3 <br>
**Output:** 12 <br>
**Explanation:** After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.

**Example 2:** <br>
**Input:** cardPoints = [2,2,2], k = 2 <br>
**Output:** 4 <br>
**Explanation:** Regardless of which two cards you take, your score will always be 4.

**Example 3:** <br>
**Input:** cardPoints = [9,7,7,9,7,7,9], k = 7 <br>
**Output:** 55 <br>
**Explanation:** You have to take all the cards. Your score is the sum of points of all cards.

**Constraints:**

-   `1 <= cardPoints.length <= 105`
-   `1 <= cardPoints[i] <= 104`
-   `1 <= k <= cardPoints.length`

## Solution:

### Using two pointer
**Algorithm**
> Result can be k elements from left and 0 elements from right, Or (k-1) elements from left and 1 element from the right and so on .... till 0 elements from left and k elements from right.

- Create a pointer `left` at the start of `arr` and another pointer `right` at end of `arr`. Initialize a variable `sum`  with value 0 to store the sum of `k` elements.
- Increment `left` to iterate over the first `k` elements and add each element to `sum`.
- Initialize integer `result` as sum (ie k elements from left and 0 elements from right).
- Now keep on removing one element from `left` and add one element from `right`. At each step compare the resultant sum with the result. Keep maximum of sum and result as result.
- Once the `arr` iteration is complete, return the `result`.

```Java
class Solution {
    public int maxScore(int[] arr, int k) {        
        int sum = 0;
        int left = 0;
        int right = arr.length - 1;
        for(; left<k; left++) {
            sum += arr[left];
        }
        left--;
        
        int result = sum;
        while(left >= 0) {
            sum = sum + arr[right] - arr[left];
            result = Math.max(result, sum);
            right--;
            left--;
        }
        
        return result;
        
    }
}
```

**Time complexity: ** `O(k)` as we need to iterate over k elements<br>
**Space complexity: ** `O(1)` as we are not creating any additional data structure<br>