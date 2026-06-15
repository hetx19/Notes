**Problem**: You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

**Example**:
**Input**: `prices = [7,1,5,3,6,4]`
**Output:** 5
**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

[Visit Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/stock-buy-and-sell-1587115621/1)

---

### Optimal Solution - [[Dynamic Programming]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxProfit(vector<int>& prices) {
		int mini = prices[0], maxprofit = 0;
		
		for (int &price : prices) {
			int profit = price - mini;
			maxprofit = max(maxprofit, profit);
			mini = min(mini, price);
		}
		
		return maxprofit;
	}
};
```
