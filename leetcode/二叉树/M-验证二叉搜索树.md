## 验证二叉搜索树

### 题目描述 中等题

[题目链接] (https://leetcode.cn/problems/validate-binary-search-tree/solutions/)

### 思路

- 两种做法
- 一是递归 二是 中序遍历
- 递归的做法：设置一个上界下界，判断子树的大小是否都在这个范围里面。
- 一开始为负无穷和正无穷
- 判断条件也变成了每个结点
- 中序遍历的做法是利用非递归形式
- 维护一个上个节点值，只要当前结点小于等于上个结点就不是
- 




    ```


    bool isValidBST(TreeNode* root) {
        //递归
        // 设置一个上界和一个下界，以便子树的所有值都能满足
        stack<TreeNode*> st;
         long long inorder = (long long)INT_MIN - 1;
        while(!st.empty() || root){
            while(root){
                st.push(root);
                root = root -> left;
            }
            TreeNode* node = st.top();
            st.pop();
            if(node->val <= inorder){
                return false;
            }
            inorder = node->val;
            root = node->right;
        }
        return true;
    }
    ```


    ```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
//必须有一个触发返回true的条件
    bool helper(TreeNode* root, long long lower, long long upper){
         //递归
        if(root == nullptr) return true;
        if (root -> val <= lower || root -> val >= upper) {
            return false;
        }
        return helper(root->left, lower, root->val) && helper(root->right, root->val, upper);
    }
    bool isValidBST(TreeNode* root) {
       return helper(root, LONG_MIN, LONG_MAX);
    }
};

    ```
