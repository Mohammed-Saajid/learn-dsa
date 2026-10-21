# Same Tree

[**LC:100**](https://leetcode.com/problems/same-tree/description/)

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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == NULL && q == NULL) {
            return true;
        }
        if (p == NULL || q == NULL) return false;
        bool cond = (p->val) == (q->val);
        if (cond) {
            return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
        }
        return false;
    }
};

```

We check if each node is same recursively.