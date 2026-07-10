**Problem**: We have a **horizontal** number line. On that number line, we have gas **stations** at positions `stations[0], stations[1], ..., stations[n-1]`. Now, we add **k** more gas stations so that **d**, the maximum distance between adjacent gas stations, is minimized. We have to find the smallest possible value of d. Find the answer **exactly** to 6 decimal places.  
**Note**: **`stations`** is in a **strictly increasing** order.

**Example**:
**Input**: `stations[] = [1, 2, 3, 4, 5], k = 2`
**Output**: 1.00
**Explanation**: Since all gaps are already equal (1 unit each), adding extra stations in between does not reduce the maximum distance.

[Visit GFG](https://www.geeksforgeeks.org/problems/minimize-max-distance-to-gas-station/1)

---
### Brute Force Solution
By using Linear Search function - [[Linear Search]]

**Time Complexity**: O(kn)
**Space Complexity**: O(n - 1) → O(n)

```cpp
class Solution {
public:
    double minMaxDist(vector<int> &stations, int K) {
        int n = stations.size();
        vector<int> howMany(n - 1, 0);

        for (int gasStation = 1; gasStation <= K; gasStation++) {
            double maxSection = -1;
            int maxIndex = -1;

            for (int i = 0; i < n - 1; i++) {
                int diff = stations[i + 1] - stations[i];
                double sectionLength = (double)diff / (howMany[i] + 1);

                if (sectionLength > maxSection) {
                    maxSection = sectionLength;
                    maxIndex = i;
                }
            }

            howMany[maxIndex]++;
        }

        double maxAns = -1;

        for (int i = 0; i < n - 1; i++) {
            int diff = stations[i + 1] - stations[i];
            double sectionLength = (double)diff / (howMany[i] + 1);
            maxAns = max(maxAns, sectionLength);
        }

        return maxAns;
    }
};
```

---

### Better Approach
By using a Priority Queue - [[Priority Queue]]

**Time Complexity**: O((n + k) log n)
**Space Complexity**: O(2 * (n - 1)) → O(n) // vector + priority Queue

```cpp
using ld = long double;

class Solution {
  public:
    double minMaxDist(vector<int> &stations, int K) {
	    int n = stations.size();
	    vector<int> howMany(n - 1, 0);
	    
	    priority_queue<pair<ld, int>> pq;
	    
	    for (int i = 0; i < n - 1; i++) {
		    pq.push({stations[i + 1] - stations[i], i});
	    }
	    
	    for (int gasStation = 1; gasStation <= K; gasStation++) {
		    auto it = pq.top();
		    pq.pop();
		    
		    int secIndex = it.second;
		    howMany[secIndex]++;
		    
		    ld inDiff = stations[secIndex + 1] - stations[secIndex];
		    ld newSection = inDiff / ((ld)howMany[secIndex] + 1);
		    pq.push({newSection, secIndex});
	    }
	    
	    return pq.top().first;
    }
};
```

---
### Optimal Solution
By using Binary Search Function - [[Binary Search]]

**Time Complexity**: O(n log (length of ans)) + O(n)
**Space Complexity**: O(1)

```cpp
using ld = long double;

class Solution {
  private:
	int numberOfGasStationsRequired(vector<int>& stations, ld distance) {
		int n = stations.size(), count = 0;
		
		for (int i = 0; i < n - 1; i++) {
			int numbersInBetween = (stations[i + 1] - stations[i]) / distance;
			
			if ((stations[i + 1] - stations[i]) == (distance * numbersInBetween)) {
				numbersInBetween--;
			}
			
			count += numbersInBetween;
		}
		
		return count;
	}
	
  public:
	double minMaxDist(vector<int> &stations, int K) {
		int n = stations.size();
		ld low = 0, high = 0;
		
		for (int i = 0; i < n - 1; i++) {
			high = max(high, (ld)(stations[i + 1] - stations[i]));
		}
		
		ld diff = 1e-6;
		while (high - low > diff) {
			ld mid = low + ((high - low) / 2);
			int count = numberOfGasStationsRequired(stations, mid);
			
			if (count > K) {
				low = mid;
			} else {
				high = mid;
			}
		}
		
		return high;
	}
};
```

---
