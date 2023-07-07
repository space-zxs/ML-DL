## 分割等和子集

### 题目描述

[题目连接](https://leetcode.cn/problems/partition-equal-subset-sum/description/)

### 思路

  - 动态规划
  - 经典背包问腿
  - 选取数组中的一部分元素等于总量的一半
  - 设定dp[i][j] 表示前i个元素能否达到j
  - 转移方程 dp[i][j] |= (dp[i-1][j] ||dp[i-1][j-nums[i]])
---

```
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        //分割等和子集
        //首先是不是2的倍数
        //典型的背包问题，即从数组中选取k个等于一个总量
        if(nums.size() < 2) return false;
        sort(nums.begin(), nums.end());
        int sum = 0;
        for(auto& num:nums){
            sum += num;
        }
        if(sum%2) return false;

        sum /= 2; 
        //动态规划
        //dp[i][j] 表示从数组的 [0,i] 下标范围内选取若干个正整数（可以是 0 个），是否存在一种选取方案使得被选取的正整数的和等于 j。
        //d[i][0] = true dp[0][nums[0]] = true
        // j >= nums[j] dp[i-1][j] || dp[i-1][j-nums[j]]        // j < nums[j] dp[i-1][j]
        vector<vector<int>> dp(nums.size(), vector<int>(sum+1));
        dp[0][0]  = 1;
        dp[0][nums[0]] = 1;
        for(int i = 1; i < nums.size(); i++){
            dp[i][0] = 1;
            for(int j = 0; j < sum + 1; j++){
                if(j >= nums[i]){
                    dp[i][j] |= (dp[i-1][j] || dp[i-1][j-nums[i]]);
                }else{
                    dp[i][j] |=dp[i-1][j];
                }
               
            }
        }
        return dp[nums.size()-1][sum];
    }
};

```
