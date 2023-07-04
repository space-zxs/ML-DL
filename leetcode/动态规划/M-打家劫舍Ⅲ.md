## 打家劫舍Ⅲ

### 题目连接
[题目连接]( https://leetcode.cn/problems/house-robber-iii/submissions/444284522/)


### 思路
- 动态规划
- 树形dp
- 设置两个状态 选中和没选中
- 进行后序遍历的递归
- 如果当前结点没选中 = 左子节点选中和没选中的最大值  + 右子节点选中和没选中的最大值
- 如果当前结点选中 = 左子节点没选中 + 当前结点值 + 右子节点没选中
- 返回 当前结点选中和没选中的值

 ---

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
struct SubtreeStatus{
    int selected;
    int notselected;
};

class Solution {
public:
    SubtreeStatus dfs(TreeNode* node){
        if(!node) return {0,0};
        SubtreeStatus l = dfs(node->left);
        SubtreeStatus r = dfs(node->right);
        int  selected = l.notselected + node->val + r.notselected;
        int notselected = max(l.notselected, l.selected) + max(r.notselected, r.selected);
        return {selected, notselected};
    }
    int rob(TreeNode* root) {
        //树形dp
        //select 选择该结点的总价值
        // no select 不选择该节点的总价值
        auto res = dfs(root);
        return max(res.notselected, res.selected);
    }
};
```
