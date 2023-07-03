## 最佳买卖股票时机含冷冻期

### 题目链接

[题目连接] (https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

### 思路：
  - 动态规划
  - 定义dp[i][j] 表示前i天的最大利润， j 有三个取值，0 代表当天不持有、不处于冷冻期
  - 1 表示当前持有一只
  - 2表示当前不持有 处于冷冻期


---

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        //动态规划
        //dp[i][j] 表示前i天的最大利润，j = 0 表示不持有不处于冷冻期，j=1表示持有一只 j = 2 不持有处于冷冻期
        //dp[i][0] = max(dp[i-1][1] + nums[i], dp[i-1][0])

        vector<vector<int>> dp(prices.size() + 1, vector<int> (3));
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        dp[0][2] = 0;
        for(int i = 1; i < prices.size(); i++){
            dp[i][0] = max(dp[i-1][2], dp[i-1][0]);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]);
            dp[i][2] = dp[i-1][1] + prices[i];
        }
        return max(dp[prices.size()-1][0], dp[prices.size()-1][2]);
    }
};
```
