## 最长连续序列

### 题目描述

[题目描述](https://leetcode.cn/problems/longest-consecutive-sequence/)

### 思路：
- 搜索每个数的上下位的数，
- 使用集合初始化所有数为1
- 遍历数组，当数组中没有这个值的小1的值，那么开始累加，并更新最大值。

---
代码

```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> st;
        for(auto &num:nums){
            st.insert(num);
        }
        int mx = INT_MIN;
        if(nums.size() == 0) return 0;
        for(int i = 0; i < nums.size(); i++){
            if(!st.count(nums[i]-1)){
              int currentnum = nums[i];
              int len = 1;
              while(st.count(currentnum+1)){
                len++;
                currentnum++;
              }
              mx = max(mx, len);
            }

        }
        return mx;
    }
};


```
