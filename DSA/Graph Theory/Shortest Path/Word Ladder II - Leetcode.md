**Problem**: A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _all the **shortest transformation sequences** from_ `beginWord` _to_ `endWord`_, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words_ `[beginWord, s1, s2, ..., sk]`.

**Example**:

**Input:** `beginWord = "hit"`, `endWord = "cog"`, `wordList = ["hot","dot","dog","lot","log","cog"]`
**Output:** `[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]`
**Explanation:** There are 2 shortest transformation sequences:
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"

[Visit Leetcode](https://leetcode.com/problems/word-ladder-ii/)

---

### Cp Optimisation

**Time complexity**: O(can't be predicted)
**Space complexity**: O(can't be predicted)

```cpp
class Solution {
	unordered_map<string, int> mpp;
	vector<vector<string>> ans;
	string b;
	
  private:
	void dfs(string word, vector<string> &seq) {
		if (word == b) {
			reverse(seq.begin(), seq.end());
			ans.push_back(seq);
			reverse(seq.begin(), seq.end());
			return;
		}
		
		int steps = mpp[word];
		int n = b.size();
		
		for (int i = 0; i < n; i++) {
			char original = word[i];
			for (char ch = 'a'; ch <= 'z'; ch++) {
				word[i] = ch;
				
				if (mpp.find(word) != mpp.end() && mpp[word] + 1 == steps) {
					seq.push_back(word);
					dfs(word, seq);
					seq.pop_back();
				}
			}
		
			word[i] = original;
		} 
	}
	
  public:
	vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
		queue<string> q;
		b = beginWord;
		unordered_set<string> st(wordList.begin(), wordList.end());
		
		q.push(beginWord);
		mpp[beginWord] = 1;
		
		int n = beginWord.size();
		st.erase(beginWord);
		
		while (!q.empty()) {
			string word = q.front();
			int steps = mpp[word];
			q.pop();
			
			if (word == endWord) {
				break;
			}
			
			for (int i = 0; i < n; i++) {
				char original = word[i];
				
				for (char ch = 'a'; ch <= 'z'; ch++) {
					word[i] = ch;
					if (st.find(word) != st.end()) {
						q.push(word);
						st.erase(word);
						mpp[word] = steps + 1;
					}
				}
				
				word[i] = original;
			}
		}
		
		if (mpp.find(endWord) != mpp.end()) {
			vector<string> seq;
			seq.push_back(endWord);
			dfs(endWord, seq);
		}
		
		return ans;
	}
};
```