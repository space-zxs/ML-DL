## 不同的二叉搜索树


### 题目描述

[题目链接] (https://leetcode.cn/problems/unique-binary-search-trees/)


### 思路:
- 二叉搜索树的数量 = 以某个结点为根节点的的左二叉搜索树的数量 * 右二叉搜索树的数量 然后对所有情况求和，也就是遍历所有结点。

---
代码
```
class Solution {
public:
    int numTrees(int n) {
        //dp[i] 表示长度为i的序列能构建的二叉搜索树的个数
        vector<int> dp(n + 1);
        dp[0] = 1;
        dp[1] = 1;
        for(int  i = 2; i <= n; i++){
            for(int  j = 1; j <=i; j++){
                dp[i] += dp[i-j] * dp[j-1]; // 拿出一个结点当根结点
            }
        }


        return dp[n];
    }
};
```
