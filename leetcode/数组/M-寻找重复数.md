## 寻找重复数

### 题目描述

[题目链接] (https://leetcode.cn/problems/find-the-duplicate-number/description/)

### 思路
- 快慢指针
- 对于只有两个有重复数字的数组
- i = nums[i] 这种指向方式 会出现环
- 因此 可以仿照环形链表的入环点判断来做


---

```
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        //快慢指针的思想
        // 这样重复的数组相当于有环
        int fast = 0;
        int slow = 0;
        while(1){
            fast = nums[nums[fast]];
            slow = nums[slow];
            if(fast == slow) break;
        }
        int head = 0;
        while(head != slow){
            head = nums[head];
            slow = nums[slow];

        }
        return slow;
    }
};

```
