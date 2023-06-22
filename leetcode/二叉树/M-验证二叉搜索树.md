## 验证二叉搜索树

### 题目描述 中等题

### 思路






    ```

// bool isValidBST(TreeNode* root) {
    //     
    //     //使用中序遍历验证，如果当前值小于前一个那么不是
    //     stack<TreeNode*> st;
    //      long long inorder = (long long)INT_MIN - 1;
    //     while(!st.empty() || root){
    //         while(root){
    //             st.push(root);
    //             root = root -> left;
    //         }
    //         TreeNode* node = st.top();
    //         st.pop();
    //         if(node->val <= inorder){
    //             return false;
    //         }
    //         inorder = node->val;
    //         root = node->right;
    //     }
    //     return true;
    // }


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
