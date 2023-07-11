## 剪绳子

### 题目描述

[剪绳子1] (https://leetcode.cn/problems/jian-sheng-zi-lcof/description/)
[整数规划](https://leetcode.cn/problems/jian-sheng-zi-lcof/description/)
[剪绳子2] （https://leetcode.cn/problems/jian-sheng-zi-lcof/description/）


### 思路：
 - 剪绳子1
 - 动态规划
 - 注意dp的设置
 - 设置dp[i] 表示和为i的两个整数的最大乘积
---
 - 剪绳子2
 - 大数越界问题

---
代码
```
class Solution {
public:
     long  mod = 1e9+7;
    int cuttingRope(int n) {
        //数学的方法

        if(n <= 3) return n- 1;
        long res = 1;
        while(n > 4){
            res = (res * 3) % mod;
            n -= 3;
        }
        return (res * n) % mod; 
    }
};

```
