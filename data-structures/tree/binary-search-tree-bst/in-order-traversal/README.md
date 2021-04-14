# In-order Traversal

* Recursion
* Iteration
  * Use stack

Example:

[Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        // sol1: recursion
//         vector<int> res;
//         helper(root, res);
//         return res;
//     }
    
//     void helper(TreeNode* node, vector<int>& res) {
//         if (!node) {
//             return;
//         }
//         helper(node->left, res);
//         res.push_back(node->val);
//         helper(node->right, res);
//     }
        
        // sol2: iteration (stack)
        stack<TreeNode*> st;
        vector<int> res;
        TreeNode* cur = root;
        while (cur || !st.empty()) {
            while (cur) {
                st.push(cur);
                cur = cur->left;
            }
            auto t = st.top();
            st.pop();
            res.push_back(t->val);
            cur = t->right;
        }
        return res;
    }
};
```

