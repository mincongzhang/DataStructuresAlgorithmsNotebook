### Best Time to Buy and Sell Stock

Say you have an array for which the ith element is the price of a given stock on day i.  
If you were only permitted to complete at most one transaction \(ie, buy one and sell one share of the stock\), design an algorithm to find the maximum profit.

e.g.  
Input: \[7, 1, 5, 3, 6, 4\]  
Output: 5  
max. difference = 6-1 = 5 \(not 7-1 = 6, as selling price needs to be larger than buying price\)

Input: \[7, 6, 4, 3, 1\]  
Output: 0  
In this case, no transaction is done, i.e. max profit = 0.

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int min_price  = INT_MAX;
        int max_profit = INT_MIN;
        for(int i=0; i<prices.size(); ++i){
            min_price = min(min_price, prices[i]);
            int profit = prices[i] - min_price;
            max_profit = max(max_profit,profit);
        }
        
        return max_profit;
    }
};
```



