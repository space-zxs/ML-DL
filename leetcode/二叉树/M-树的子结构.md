## 树的子结构

### 题目描述

[题目连接](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/description/)


### 思路：
 - 递归的思想
 - 对A树进行递归，从每个结点开始判断
 - 如果结点相等
 - 对AB两棵树同时递归，满足条件返回true
 - 是一种自上而下的递归方式

---

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        // B是A的子结构的两种情况
        // B 是空的说明暂时是A的子树、B是A的一部分则可以继续递归检查 \ A是空返回false
        return  (A!=nullptr && B != nullptr) && (recur(A, B) || isSubStructure(A->left, B) || isSubStructure(A->right,B)); 
    }
    bool recur(TreeNode* A, TreeNode* B){
        if(B == nullptr)  return true;
        if(A == nullptr || A->val != B->val)  return  false;
        return recur(A->left, B->left) && recur(A->right, B->right); 
    }
};

```
