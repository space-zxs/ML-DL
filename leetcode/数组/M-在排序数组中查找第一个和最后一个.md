## 在排序数组中查找第一个和最后一个元素

### 中等题，题目描述
[题目链接] (https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

### 思路：
- 因为要求logn的时间复杂度因此使用二分法
- 两个二分法分别查找左边界和右边界
- 加入一个独特的符号判断是在找左边界还是有边界
- 在循环中当==target时，如果寻找左边界那么，r-1.寻找右边界，l+1
- 这时候就相当于正常的二分查找，
- 最后 l 和r的值都是边界旁边的值
- 可以单独设置参数记录，边界值，初始化为-1


---
```
class Solution {
public:
    int binary_search(vector<int>& nums,int l, int r,int target, int flag){
       int ans_l = -1;
       int ans_r = -1;
       while(l <= r){
           int m = (l + r) >> 1;
           if(nums[m] < target){
               l = m + 1;
           }else if(nums[m] > target){
               r = m -1;
           }else{
               if(flag){
                   ans_l = r;
                   r = r - 1;
               }else{
                   ans_r = l;
                   l = l + 1;
               }
           }
       }

       if(flag) return ans_l;
       return ans_r;
    }
    vector<int> searchRange(vector<int>& nums, int target) {
        //实现log n 复杂度, 唯有二分法
        /**
        思路：
            先用一个二分查找target左边界
            再用一个二分查找target右边界
            二分查找的一些注意细节   
        */
        int l = 0;
        int r = nums.size() -1;
        int left = binary_search(nums,l, r, target,1);
        int right = binary_search(nums, l, r,target, 0);
        return {left, right};

    }
};

```
