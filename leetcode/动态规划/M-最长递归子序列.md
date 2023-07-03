## 最长递归子序列

### 题目描述


[题目链接](https://leetcode.cn/problems/longest-increasing-subsequence/submissions/443957810/)

### 思路：
- 动态规划
- 容易想到最长递归子序列，应该是前面最长的子序列+1 （要满足当前值大于前面比较的值）
- 所有dp[i] 为前i个元素并且以第i个元素结尾的最长递增子序列的长度
- dp[i] = max(dp[0...i-1]) + 1


---

```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        //定义dp[i] 为前 i 个元素，以第i个元素结尾的最长上升子序列的长度
        //dp[i] = max(dp[0...i-1]) + 1; (要求 nums[i] > nums[j])
        vector<int> dp(nums.size() + 1);
        dp[0] = 1;
        int mx = 1;
        for(int i = 1; i < nums.size(); i++){
            int mj = 0;
            for(int j = 0; j <i; j++ ){
                if(nums[j] < nums[i]){
                    mj = max(mj, dp[j]);
                }
                dp[i] = mj  + 1;
                mx = max(mx, dp[i]);
            }
        }
        return mx;
    }
};
```
