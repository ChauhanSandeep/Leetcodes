## 1710. Maximum Units on a Truck (30/06/2022)

## Problem:
You are assigned to put some amount of boxes onto  **one truck**. You are given a 2D array  `boxTypes`, where  `boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi]`:

-   `numberOfBoxesi`  is the number of boxes of type  `i`.
-   `numberOfUnitsPerBoxi`  is the number of units in each box of the type  `i`.

You are also given an integer  `truckSize`, which is the  **maximum**  number of  **boxes**  that can be put on the truck. You can choose any boxes to put on the truck as long as the number of boxes does not exceed  `truckSize`.

Return  _the  **maximum**  total number of  **units**  that can be put on the truck._

**Example 1:** <br>
**Input:** boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4 <br>
**Output:** 8 <br>
**Explanation:** There are: <br>
- 1 box of the first type that contains 3 units.
- 2 boxes of the second type that contain 2 units each.
- 3 boxes of the third type that contain 1 unit each.
You can take all the boxes of the first and second types, and one box of the third type.
The total number of units will be = (1 * 3) + (2 * 2) + (1 * 1) = 8.

**Example 2:** <br>
**Input:** boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10 <br>
**Output:** 91

**Constraints:** <br>
-   `1 <= boxTypes.length <= 1000`
-   `1 <= numberOfBoxes, numberOfUnitsPerBox <= 1000`
-   `1 <= truckSize <= 106`

## Solution:

### Sorting Array: 
**Algorithm**:
- Sort array `boxTypes` such that boxType with max units per box are first in the array.
- Start consuming boxes from starting of the array. Reduce truckSize according to the boxes consumed and add units to result `maxUnits`.

```Java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        Arrays.sort(boxTypes, (a, b) -> b[1] - a[1]);
        
        int maxUnits = 0;
        int index = 0;
        while(truckSize > 0 && index < boxTypes.length) {
            int boxes = boxTypes[index][0];
            int unitsPerBox = boxTypes[index][1];
            
            if(boxes > truckSize) {
                boxes = truckSize;
            }
            maxUnits += (boxes * unitsPerBox);
            
            truckSize -= boxes;
            index++;
        }
        
        return maxUnits;
    }
}
```
**Time Complexity: ** `O(n log n) + O(n)` where `n` is length of array. This is required for sorting and iterating through array <br>
**Space Complexity: ** `O(1)` as we don't need any extra data structure <br>

### Creating boxes bucket: 
**Algorithm**:
- As constraint is provided `1 <= boxTypes.length <= 1000` we can create bucket to store `numberOfBoxes` on `unitPerBox`.
- Start iterating from end of the `bucket` so we take more number of units per box.
- On each index reduce the number of units from `truckSize` and add total units.

```Java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        int[] bucket = new int[1001];
        int maxUnits = 0;
        
        for(int[] boxType : boxTypes) {
            int boxes = boxType[0];
            int unitPerBox = boxType[1];
            bucket[unitPerBox] += boxes;
        }
        for(int i = 1000; i >= 0 && truckSize > 0; i--) {
            if(bucket[i] == 0) continue;
            
            int units = Math.min(truckSize, bucket[i]);
            truckSize -= units;
            maxUnits += (units * i);
        }
        return maxUnits;
    }
}
```
**Time Complexity: ** `O(n) + O(1001) = O(n)` Where n is length of array.  we need to iterate through array `boxTypes` once and then we need to iterate over `bucket` of constant size(hence ignored)<br>
**Space Complexity: ** `O(1001) = O(1)` as we need constant extra space for array `bucket`<br>