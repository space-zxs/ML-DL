## 最大子数组和

### 题目描述
[题目链接](https://leetcode.cn/problems/maximum-subarray/submissions/441464885/)

### 思路：
  - 最大子数组
  - 动态规划
  - 设置一个最大值变量更新
  - dp[i] 表示前i个数字的最大和
  - dp[i] = max(nums[i], dp[i-1] + nums[i])
  - 最大值初始化为nums[0]的值。

  ---
  代码
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        //动态规划
        //dp[i] 表示i之前的最大连续数字和
        //dp[i] = max(nums[i], dp[i-1] + nums[i]);
        vector<int> dp(nums.size());\
        int mx = nums[0];
        dp[0] = nums[0];
        for(int i = 1; i < nums.size(); i++){
            dp[i] = max(nums[i], dp[i-1] + nums[i]);
            mx = max(mx, dp[i]);
        }
        return mx;
    }
};
```
  
