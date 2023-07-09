## 和为K的子数组

### 题目描述
[前缀和问题](https://leetcode.cn/problems/subarray-sum-equals-k/solutions/562174/de-liao-yi-wen-jiang-qian-zhui-he-an-pai-yhyf/)

[和为k的子数组]()


### 思路：
  - 双重循环直观容易想到n2的复杂度
  - 前缀和进行优化
  - 主要思想是  prei - prej = k
  - 存储一个前缀和，维护一个哈希表
  - 寻找pre - k在哈希表的值也就是有几个和为k
---
双重循环
```
class Solution {
    public int subarraySum(int[] nums, int k) {
         int len = nums.length;
         int sum = 0;
         int count = 0;
         //双重循环
         for (int i = 0; i < len; ++i) {
             for (int j = i; j < len; ++j) {
                 sum += nums[j];
                 //发现符合条件的区间
                 if (sum == k) {
                     count++;
                 }
             }
             //记得归零，重新遍历
             sum = 0;
         }
         return count;
    }
}

作者：程序厨
链接：https://leetcode.cn/problems/subarray-sum-equals-k/solutions/562174/de-liao-yi-wen-jiang-qian-zhui-he-an-pai-yhyf/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```
    ---
前缀和优化
     ```
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int i = 0, j = 0;
        //int sum = nums[0];
        int count = 0;
        int pre =0;
       
        unordered_map<int,int> mp;
        mp[pre]++;
        for(auto& num : nums){
            pre += num;
            if(mp.find(pre - k) != mp.end()){
                count+=mp[pre-k];
            }
            mp[pre]++;
        }
        return count;
    }
};

     ```
