## 1647. Minimum Deletions to Make Character Frequencies Unique

## Problem:

A string  `s`  is called  **good**  if there are no two different characters in  `s`  that have the same  **frequency**.

Given a string  `s`, return _the  **minimum**  number of characters you need to delete to make_ `s` _**good**._

The  **frequency**  of a character in a string is the number of times it appears in the string. For example, in the string  `"aab"`, the  **frequency**  of  `'a'`  is  `2`, while the  **frequency**  of  `'b'`  is  `1`.

**Example 1:** <br>
**Input:** s = "aab" <br>
**Output:** 0 <br>
**Explanation:** `s` is already good.

**Example 2:** <br>
**Input:** s = "aaabbbcc" <br>
**Output:** 2 <br>
**Explanation:** You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".

**Example 3:** <br>
**Input:** s = "ceabaacb" <br>
**Output:** 2 <br>
**Explanation:** You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).

**Constraints:**
-   `1 <= s.length <= 105`
-   `s` contains only lowercase English letters.

## Solution:

```Java
class Solution {
    public int minDeletions(String str) {
        // create array containing frequency of each character
        int[] freqArr = new int[26];
        for(char c: str.toCharArray()) {
            freqArr[c - 'a']++;
        }
        
        // sort frequency array
        Arrays.sort(freqArr);
        
        // traverse in reverse order and calculate deletions as freqArr[i] - maxAllowedFreq
        int deletions = 0;
        int maxAllowedFreq = str.length();
        
        for(int i=freqArr.length - 1; i>=0; i--) {
            if(freqArr[i] > maxAllowedFreq) {
                deletions += (freqArr[i] - maxAllowedFreq);
                freqArr[i] = maxAllowedFreq;
            }
            maxAllowedFreq = freqArr[i] - 1;
            if(maxAllowedFreq < 0) maxAllowedFreq = 0;
        }
        
        return deletions;
        
    }
}
```

**Time Complexity: ** `O(N)` where N is the length of the given string. Sorting can be ignored as the array size will always be 26. <br>
**Space Complexity: ** `O(1)` as array size is always 26, hence it can be ignored.