## 二叉树的最近公共祖先

### 题目描述

[题目链接](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solutions/240096/236-er-cha-shu-de-zui-jin-gong-gong-zu-xian-hou-xu/)

### 思路
- 递归
- 两种情况
- p，q分别在左右两颗子树上，或者 root == p 或者 root == q，另一个一定在一颗子树上 此时就是root
- 返回左右子树上，l || r || root==p || root==q


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
    TreeNode* ans;
    bool dfs(TreeNode* root, TreeNode* p, TreeNode* q){
        if(!root) return false;
        bool l = dfs(root->left, p, q);
        bool r = dfs(root->right, p, q);
        if((l&&r) || ((root->val == p-> val || root->val == q->val) && (l || r))){
            ans = root;
        }
        return l || r || ( root->val == p->val || root->val == q->val);
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        dfs(root, p, q);
        return ans;
    }
};

  ```
