## Diameter of a Binary tree
[**LC: 543**](https://leetcode.com/problems/diameter-of-binary-tree/description/)

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
    int max_diameter = 0;
    int height(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        int left_subtree = height(root->left);
        int right_subtree = height(root->right);
        max_diameter = max(max_diameter, left_subtree + right_subtree);
        return max(left_subtree,right_subtree) + 1;
        
    }
    int diameterOfBinaryTree(TreeNode* root) {
        height(root);
        return max_diameter;
    }
};
```

Ideally, The problem is to find the longest available path from one node to another node, So initial assumption will be to find the height of the left sub tree and right sub tree and add that, but that will not be the case always, Because we cannot be certain that always we will get the middlest node as the root node, what if we get a leaf node as a root node?. So for that cases, we are gonna keep track of maximum of left_subtree + right_subtree heights respectively for each node so that eventually the middlessttt node's value will be the maximum i mean that is the middlesstt in the path.