# Invert a Binary Tree

[**LC: 226**](https://leetcode.com/problems/invert-binary-tree/description/)

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
    TreeNode* invertTree(TreeNode* root) {
        // Base Case
        if (root == NULL) {
            return NULL;
        }
        invertTree(root->left); // call the left subtree
        invertTree(root->right); // call the right subtree
        auto temp =  root->left;
        root->left = root->right;
        root->right = temp;
        return root;
    }
};
```

## 1. Problem Explanation

The problem is to invert a binary tree, meaning that for every node in the tree, its left and right children must be swapped. This operation must be performed recursively for all nodes in the tree, including those in the left and right subtrees. If the tree is empty (`root == nullptr`), the function should return `nullptr`. Otherwise, it should return the root of the inverted tree.

## 2. Core Idea

The solution uses recursion to invert the binary tree. First, check whether the current node is `nullptr`; if so, return immediately because there is nothing to invert. Otherwise, recursively invert the left subtree and the right subtree. Once both recursive calls are completed, swap the current node's left and right child pointers using a temporary variable. This process continues until every node has been visited and its children have been swapped. Finally, return the original root pointer, which now represents the inverted tree.

## 3. Implementation

Time Complexity: `O(n)`, where nnn is the number of nodes in the tree, because each node is visited exactly once.

Space Complexity: `O(h)`, where hhh is the height of the tree, due to the recursive call stack. In a balanced tree, this is `Olog(n)`; in a completely skewed tree, it is `O(n)`

Key takeaway: Recursively invert both subtrees, swap the current node's children, and return the root. This produces the mirror image of the original binary tree without creating any new nodes.
