# Balanced Binary Tree
[**LC:110**](https://leetcode.com/problems/balanced-binary-tree/description/)

```cpp
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
    int height(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        int left_height = height(root->left);
        int right_height = height(root->right);
        return 1 + max(left_height,right_height);
    }
    bool isBalanced(TreeNode* root) {
        if (root==NULL) {
            return true;
        }
        int diff = (height(root->left)-height(root->right));
        bool isbalanced = (diff <=1) && (diff >=-1);
        return isbalanced && isBalanced(root->left) && isBalanced(root->right);
    }
};

```

It's the same height measuring problem but we just check the difference between the subtree's heights at each node, just check the recursive return logic which is pretty new.
