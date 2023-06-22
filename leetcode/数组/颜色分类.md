## 颜色分类


### 题目描述


[题目链接](https://leetcode.cn/problems/sort-colors/solutions/437968/yan-se-fen-lei-by-leetcode-solution/)

### 思路：
  - 计数排序，对于这个排序需要两次遍历，先记录每个数字出现的个数，然后重写覆盖原数组
  - 题目要求一次遍历
  - 使用双指针，调换元素
  - 设置初始指针p0和p1初始都是0
  - p0用于记录0，p1用于记录1
  - 写一个循环
  - 判断当前值是不是1，如果是1，交换p1的位置和当前位置值，p1++
  - 如果是0，那么交换p0和当前位置，如果p0 《 p1 一定交换了一个1.因此交换当前位置和p1
  - 不论是不是p0小于p1，都会把两个指针向后移动一位，保证1出现在0后面。

---

```
class Solution {
public:
    void sortColors(vector<int>& nums) {
        //双指针处理，一个p0用于处理0，一个p1用于处理1
        // 注意两个指针初始化都为1，当交换0时有可能会交换1，这时需要把1放到头部的末端
        //p0 和 p1都移动一个位置

        int p0 = 0;
        int p1 = 0;
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] == 1){
                swap(nums[i], nums[p1]);
                p1++;
            }else if(nums[i] == 0 ){
                swap(nums[i], nums[p0]);
                
                //这时如果p0小于p1，一定会把一个1交换出去
                if(p0 < p1){
                    //把这个交换出去的1 放p1处
                    swap(nums[i], nums[p1]);
                    
                }
                //同时移动保持1在0后面
                p1++;
                p0++;
                
            }
        }
    }
};
```
    
  
