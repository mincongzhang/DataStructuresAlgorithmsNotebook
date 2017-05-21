### Best Time to Buy and Sell Stock III

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

Note:  
You may not engage in multiple transactions at the same time \(ie, you must sell the stock before you buy again\).

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/\#/description](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/#/description)

```
//Dynamic Programming

typedef unsigned int uint;

class Solution {
private:
    int getProfit(const vector<int>& prices, uint begin, uint transaction){
        if(transaction>=2) return 0;
        if(begin >= prices.size()) return 0;

        int min_price = INT_MAX;
        int max_profit = INT_MIN;

        for(uint i=begin; i<prices.size(); ++i){
            min_price = min(min_price,prices[i]);

            int cur_profit = prices[i] - min_price;

            //get next max profit
            if(cur_profit > 0){
                cur_profit += getProfit(prices, i+1, transaction+1);
            }
            if(cur_profit > max_profit){
                max_profit = cur_profit;
            }
        }

        if(max_profit < 0) return 0;
        return max_profit;
    }

public:
    int maxProfit(vector<int>& prices) {
        return getProfit(prices, 0, 0);
    }
};
```

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int local_min1(INT_MIN), local_min2(INT_MIN);
        int max_profit1(0), max_profit2(0);

        for(int i=0; i<prices.size(); ++i){
            local_min1  = max(local_min1, -prices[i]);
            max_profit1 = max(max_profit1,local_min1+prices[i]);

            local_min2  = max(local_min2,  max_profit1-prices[i]);
            max_profit2 = max(max_profit2,local_min2+prices[i]);
        }

        return max_profit2;
    }
};
```



