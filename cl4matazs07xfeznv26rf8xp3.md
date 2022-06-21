## 820. Short Encoding of Words (20/06/2022)

### Problem:
A **valid encoding** of an array of `words` is any reference string `s` and array of indices `indices` such that:

-   `words.length == indices.length`
-   The reference string `s` ends with the `'#'` character.
-   For each index `indices[i]`, the **substring** of `s` starting from `indices[i]` and up to (but not including) the next `'#'` character is equal to `words[i]`.

Given an array of `words`, return _the **length of the shortest reference string**_ `s` _possible of any **valid encoding** of_ `words`_._

**Example 1:**

**Input:** words = ["time", "me", "bell"]

**Output:** 10

**Explanation:** A valid encoding would be s = `"time#bell#" and indices = [0, 2, 5`].
words[0] = "time", the substring of s starting from indices[0] = 0 to the next '#' is underlined in "time#bell#"
words[1] = "me", the substring of s starting from indices[1] = 2 to the next '#' is underlined in "time#bell#"
words[2] = "bell", the substring of s starting from indices[2] = 5 to the next '#' is underlined in "time#bell#"

**Example 2:**

**Input:** words = ["t"]

**Output:** 2

**Explanation:** A valid encoding would be s = "t#" and indices = [0].

**Constraints:**

-   `1 <= words.length <= 2000`
-   `1 <= words[i].length <= 7`
-   `words[i]` consists of only lowercase letters.

### Solution:
#### Using Set
**Algorithm**
> Words which are suffix of another word in the array can be ignored as they will be covered as part of bigger word

- Store all words in set
- For each word check if there is any suffix present in set. If yes, then remove suffix word.
- Calculate length of encoded string by using remaining words in set.

```Java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        Set<String> set = new HashSet<>();
        // put words in set
        for(String word: words) {
            set.add(word);
        }
        
        // remove the words which are suffix of some other word
        for(String word: words) {
            for(int i=1; i<word.length(); i++) {
                set.remove(word.substring(i));
            }
        }
        
        // add length of remaining words
        int result = 0;
        for(String word: set) {
            result += word.length() + 1;
        }
        return result;
        
    }
}
```

#### Using Trie
**Algorithm**
- Insert each word into trie in reverse order so that suffixes are ignored.
- Traverse through each word in trie and add `length + 1` to find total length of encoded string.

``` Java
class Solution {
    private int result;
    
    public int minimumLengthEncoding(String[] words) {
        this.result = 0;
        Trie trie = new Trie();
        
        for(String word: words) {
            trie.reverseInsert(word);
        }
        
        dfs(trie.root, 0);
        return result;
    }
    
    private void dfs(TrieNode node, int length) {
        if(node == null) return;
        boolean containsChild = false;
        
        for(TrieNode child: node.children) {
            if(child != null) {
                containsChild = true;
                dfs(child, length+1);
            }
        }
        
        if(!containsChild) result += length + 1;
    }
}

class Trie{
    TrieNode root;
    
    public Trie() {
        this.root = new TrieNode(' ');
    }
    
    public void insertWord(String word) {
        TrieNode curr = root;
        for(char c: word.toCharArray()) {
            if(curr.children[c - 'a'] == null) {
                curr.children[c - 'a'] = new TrieNode(c);
            }
            curr = curr.children[c - 'a'];
        }
    }
    
    public void reverseInsert(String word) {
        TrieNode curr = root;
        for(int i=word.length() - 1; i>=0; i--) {
            char c = word.charAt(i);
            if(curr.children[c - 'a'] == null) {
                curr.children[c - 'a'] = new TrieNode(c);
            }
            curr = curr.children[c - 'a'];
        }
    }
    
}

class TrieNode {
    public char c;
    public TrieNode[] children = new TrieNode[26];
    
    public TrieNode(char c) {
        this.c = c;
    }
}
```