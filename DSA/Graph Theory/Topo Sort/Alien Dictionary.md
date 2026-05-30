A new alien language uses the English alphabet, but the order of letters is unknown. You are given a list of **words[]** from the alien language’s dictionary, where the words are claimed to be **sorted lexicographically** according to the language’s rules.

Your task is to determine **the correct order of letters** in this alien language based on the given words. If the order is valid, return a string containing the unique letters in lexicographically increasing order as per the new language's rules. If there are multiple valid orders, return any one of them.

However, if the given arrangement of words is inconsistent with any possible letter ordering, return an empty string **("")**.
- A string **a** is lexicographically smaller than a string **b** if, at the first position where they differ, the character in **a** appears earlier in the alien language than the corresponding character in **b*. If all characters in the shorter word match the beginning of the longer word, the shorter word is considered smaller.

**Note:** Your implementation will be tested using a driver code. It will print **true** if your returned order correctly follows the alien language’s lexicographic rules; otherwise, it will print **false**.

**Examples:**

**Input:** words[] = `["baa", "abcd", "abca", "cab", "cad"]`
**Output:** true  
**Explanation:** A possible correct order of letters in the alien dictionary is "bdac".  
The pair "baa" and "abcd" suggests 'b' appears before 'a' in the alien dictionary.
The pair "abcd" and "abca" suggests 'd' appears before 'a' in the alien dictionary.
The pair "abca" and "cab" suggests 'a' appears before 'c' in the alien dictionary.
The pair "cab" and "cad" suggests 'b' appears before 'd' in the alien dictionary.
So, 'b' → 'd' → 'a' → 'c' is a valid ordering.

[Visit GFG](https://www.geeksforgeeks.org/problems/alien-dictionary/1)

---

### By Topological Sort
- **Time** Complexity: O(V + E) → (V = unique characters, E = relations)
- **Space Complexity**: O(V + E)

```cpp
class Solution {
  private:
	string topoSort(unordered_map<char, vector<char>> &adj, unordered_map<char, int> &indegree) {
		queue<char> q;
		
		for (auto it : indegree) {
			if (it.second == 0) {
                q.push(it.first);
            }
		}
		
		string topo = "";
		
		while (!q.empty()) {
            int node = q.front();
            q.pop();
            
            topo += node;
            
            for (char it : adj[node]) {
                indegree[it]--;
                if (indegree[it] == 0) {
                    q.push(it);
                }
            }
        }
        
        if (topo.size() != indegree.size()) {
            return "";
        }
		
		return topo;
	}
  
  public:
	string findOrder(vector<string> &words) {
		unordered_map<char, vector<char>> adj;
		unordered_map<char, int> indegree;
		
		for (string word : words) {
			for (char ch : word) {
				indegree[ch] = 0;
			}
		}
		
		int n = words.size();
		for (int i = 0; i < n - 1; i++) {
			string s1 = words[i], s2 = words[i + 1];
			
			int len = min(s1.size(), s2.size());
			
			if (s1.size() > s2.size() && s1.substr(0, len) == s2) {
                return "";
            }
            
            for (int ptr = 0; ptr < len; ptr++) {
	            if (s1[ptr] != s2[ptr]) {
		            bool exists = false;
		            
		            for (char x : adj[s1[ptr]]) {
                        if (x == s2[ptr]) {
                            exists = true;
                            break;
                        }
                    }
                    
                    if (!exists) {
                        adj[s1[ptr]].push_back(s2[ptr]);
                        indegree[s2[ptr]]++;
                    }
                    
                    break;
	            }
            }
		}
		
		return topoSort(adj, indegree);
	}
};
```
