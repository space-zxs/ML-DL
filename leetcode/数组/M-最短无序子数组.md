  ## 最短无序子数组

  ### 题目描述

  [题目连接] (https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/description/)

  ### 思路

   - 首先可以想到，把数组分成三部分，左边有序， 中间无序 ， 右边有序
   - 从左往右遍历寻找右边界，如果当前值小于之前的值，那么显然这个位置需要排序
   - 从右往左寻找左边界，如果当前值大于之前的值，那么显然这个位置需要排序

---

```
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        /*
        从左往右遍历，遍历到i，max记录的是从0到i最大的数，如果第i个位置比max小，证明第i位置元素处在一 个不正确的位置(因为它前面有个比它 大的数)，记录下标high。
 从右往左遍历，遍历到i，min记录的是从末尾元素到i元素最小的数，如果第i位置元素比min大了，证明第i位置元素  也处在一个不正确的位置(因为它后面有比它小的数)，记录下标low。
计算两个不正确的位置low和high之间的距离。
*/
        int mi = INT_MIN, mx = INT_MAX;
        int left = -1, right = -1;
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] < mi){
                right = i;
            }else{
                mi = nums[i];
            }

            if(nums[nums.size() -1 -i] > mx){
                left = nums.size() - i - 1;
            }else{
                mx = nums[nums.size() - i - 1];
            }
        }
        return right == -1 ? 0 : right - left + 1;
    }
};

/**
很简单，如果最右端的一部分已经排好序，这部分的每个数都比它左边的最大值要大，同理，如果最左端的一部分排好序，这每个数都比它右边的最小值小。所以我们从左往右遍历，如果i位置上的数比它左边部分最大值小，则这个数肯定要排序， 就这样找到右端不用排序的部分，同理找到左端不用排序的部分，它们之间就是需要排序的部分
*/

```
