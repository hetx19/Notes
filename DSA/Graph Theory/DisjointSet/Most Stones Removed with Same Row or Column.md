**Problem**: On a 2D plane, we place `n` stones at some integer coordinate points. Each coordinate point may have at most one stone.

A stone can be removed if it shares either **the same row or the same column** as another stone that has not been removed.

Given an array `stones` of length `n` where `stones[i] = [xi, yi]` represents the location of the `ith` stone, return _the largest possible number of stones that can be removed_.

**Example**:
![[Most Stones Removed with Same Row or Column.png|182]]

**Input**: `stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]`
**Output**: 5
**Explanation**: One way to remove 5 stones is as follows:
1. Remove stone `[2,2]` because it shares the same row as `[2,1]`.
2. Remove stone `[2,1]` because it shares the same column as `[0,1]`.
3. Remove stone `[1,2]` because it shares the same row as `[1,0]`.
4. Remove stone `[1,0]` because it shares the same column as `[0,0]`.
5. Remove stone `[0,1]` because it shares the same row as `[0,0]`.
Stone `[0,0]` cannot be removed since it does not share a row/column with another stone still on the plane.

[Visit Leetcode](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/maximum-stone-removal-1662179442/1)

---

### By using DisjointSet Data Structure - [[DisjointSet Data Structure]]

**Time Complexity**: O(V<sup>2</sup>)
**Space Complexity**: O()

```cpp
class DisjointSet {
    vector<int> parent, size;

public:
    DisjointSet(int V) {
        parent.resize(V + 1);
        size.resize(V + 1, 1);

        for(int i = 0; i <= V; i++) {
            parent[i] = i;
        }
    }

    int findParent(int node) {
        if(parent[node] == node) {
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
	int removeStones(vector<vector<int>>& stones) {
		int V = stones.size();
		int maxRow = 0, maxCol = 0;
		
		for (auto &stone : stones) {
			maxRow = max(maxRow, stone[0]);
			maxCol = max(maxCol, stone[1]);
		}
		
		unordered_map<int, int> stoneNodes;
		DisjointSet ds(maxRow + maxCol + 1);
		
		for (auto &stone : stones) {
			int nodeRow = stone[0];
			int nodeCol = stone[1] + maxRow + 1;
			
			ds.unionBySize(nodeRow, nodeCol);
			stoneNodes[nodeRow] = 1;
			stoneNodes[nodeCol] = 1;
		}
		
		int count = 0;
		for (auto &it : stoneNodes) {
			if (ds.findParent(it.first) == it.first) {
				count++;
			}
		}
		
		return V - count;
	}
};
```