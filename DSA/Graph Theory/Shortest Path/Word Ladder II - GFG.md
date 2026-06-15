**Problem**: Given two distinct words **startWord** and **targetWord**, and a list denoting **wordList** of unique words of equal lengths. Find all shortest transformation sequence(s) from startWord to targetWord. You can return them in any order possible.  
Keep the following conditions in mind:

- A word can only consist of lowercase characters.
- Only one letter can be changed in each transformation.
- Each transformed word must exist in the wordList including the targetWord.
- startWord may or may not be part of the wordList.
- Return an empty list if there is no such transformation sequence.

  
**Example**: 
**Input**: `startWord = "der", targetWord = "dfs", wordList = {"des","der","dfr","dgt","dfs"}`

**Output**: `[der dfr dfs, der des dfs`

**Explanation**:
The length of the smallest transformation is 3.
And the following are the only two ways to get
to targetWord:-
"der" -> "des" -> "dfs".
"der" -> "dfr" -> "dfs".

[Visit GFG](https://www.geeksforgeeks.org/problems/word-ladder-ii/1)

---

### By BFS Traversal + Observation 
 
**Time complexity**: O(highly dependent on input)
**Space complexity**: O(highly dependent on input)

```cpp
class Solution {
  public:
    vector<vector<string>> findSequences(string beginWord, string endWord, vector<string>& wordList) {
        queue<vector<string>> q;
        unordered_set<string> st(wordList.begin(), wordList.end());
        q.push({beginWord});
        vector<string> usedOnLevel;
        usedOnLevel.push_back(beginWord);
        int level = 0;
        
        vector<vector<string>> ans;
        
        while (!q.empty()) {
            vector<string> seq = q.front(); 
            q.pop();
            
            if (seq.size() > level) {
                level++;
                for (auto it : usedOnLevel) {
                    st.erase(it);
                }
            }
            
            string word = seq.back();
            
            if (word == endWord) {
                if (ans.size() == 0) {
                    ans.push_back(seq);
                } else if (ans[0].size() == seq.size()) {
                    ans.push_back(seq);
                }
            }
            
            for (int i = 0; i < word.size(); i++) {
                char original = word[i];
                for (char ch = 'a'; ch <= 'z'; ch++) {
                    word[i] = ch;
                    if (st.count(word) > 0) {
                        seq.push_back(word);
                        q.push(seq);
                        usedOnLevel.push_back(word);
                        seq.pop_back();
                    }
                }
                word[i] = original;
            }
        }
        
        return ans;
    }
};
```

---