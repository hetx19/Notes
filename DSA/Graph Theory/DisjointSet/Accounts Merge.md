**Problem**: Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

**Example**:

**Input** `accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]`
**Output**: `[["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]`
**Explanation**:
The first and second John's are the same person as they have the common email "johnsmith@mail.com".
The third John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer `[['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], ['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']]` would still be accepted.

[Visit Leetcode](https://leetcode.com/problems/accounts-merge/)
[Visit GFG](https://www.geeksforgeeks.org/problems/account-merge/1)

---

### By using DisjointSet - [[DisjointSet Data Structure]]

**Time Complexity**: O()
**Space Complexity**: O()

```cpp
class DisjointSet {
  public:
	vector<int> parent, size;
	
	DisjointSet(int V) {
		parent.resize(V);
		size.resize(V, 1);
		
		for (int i = 0; i < V; i++) {
			parent[i] = i;
		}
	}
	
	int findParent(int node) {
		if (node == parent[node]) {
			return node;
		}
		
		return parent[node] = findParent(parent[node]);
	}
	
	void unionBySize(int u, int v) {
		int pu = findParent(u), pv = findParent(v);
		
		if (pu == pv) {
			return;
		}
		
		if (size[pu] > size[pv]) {
			parent[pv] = pu;
			size[pu] += size[pv];
		} else {
			parent[pu] = pv;
			size[pv] += size[pu];
		}
	}
};

class Solution {
  public:
	vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
		int V = accounts.size();
		unordered_map<string, int> mapMailNode;
		
		DisjointSet ds(V);
		for (int i = 0; i < V; i++) {
			for (int j = 1; j < accounts[i].size(); j++) {
				string mail = accounts[i][j];
				
				if (mapMailNode.find(mail) == mapMailNode.end()) {
					mapMailNode[mail] = i;
				} else {
					ds.unionBySize(i, mapMailNode[mail]);
				}
			}
		}
		
		vector<vector<string>> mergedMails(V);
		
		for (auto &it : mapMailNode) {
			string mail = it.first;
			int node = ds.findParent(it.second);
			mergedMails[node].push_back(mail);
		}
		
		vector<vector<string>> ans;
		
		for (int i = 0; i < V; i++) {
			if (mergedMails[i].size() == 0) {
				continue;
			}
			
			sort(mergedMails[i].begin(), mergedMails[i].end());
			vector<string> temp;
			temp.push_back(accounts[i][0]);
			
			for (auto &it : mergedMails[i]) {
				temp.push_back(it);
			}
			
			ans.push_back(temp);
		}
		
		return ans;
	}
};
```

---