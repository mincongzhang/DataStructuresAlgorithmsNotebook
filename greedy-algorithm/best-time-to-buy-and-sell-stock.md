
### Best Time to Buy and Sell Stock
Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

e.g.
Input: [7, 1, 5, 3, 6, 4]
Output: 5
max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)

Input: [7, 6, 4, 3, 1]
Output: 0
In this case, no transaction is done, i.e. max profit = 0.

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.empty()) return 0;
        
        int max_price = 0;
        int local_min = prices[0];
        for(size_t i=1; i<prices.size(); ++i){
            local_min = std::min(prices[i],local_min);
            max_price = std::max(prices[i] - local_min,max_price); 
        }
        
        return max_price;
    }
};
```