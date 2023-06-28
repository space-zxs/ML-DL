  ## 乘积最大子数组

  ### 题目描述

  [题目链接](https://leetcode.cn/problems/maximum-product-subarray/solutions/250015/cheng-ji-zui-da-zi-shu-zu-by-leetcode-solution/)

  ### 思路
方法一：遍历

- 两个循环直接遍历所有乘积纪录最大值

方法二：动态规划

  ---

方法二的代码：
  ```
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        //动态规划
        // 只需要不断更新记录最大值和最小值,即比较维护的最大值和当前值的大小，因为是连续的
        //当遇到负数，最大值和最小值交换
        // 更新最大最小值，更新全局最大值
        int mx = INT_MIN;
        int ma = 1;
        int mi = 1;
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] < 0){
                int temp = ma;
                ma = mi;
                mi = temp;
            }
            ma = max(ma*nums[i], nums[i]);
            mi = min(mi*nums[i], nums[i]);

            mx = max(mx, ma);
        }
      return mx;
    }
};


  ```
