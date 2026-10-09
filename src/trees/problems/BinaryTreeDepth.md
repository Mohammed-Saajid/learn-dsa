# Maximum Depth of a Binary Tree
[**LC:104**](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

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
    int maxDepth(TreeNode* root) {
        if (!root) {
            return 0;
        }
        return 1 + max(maxDepth(root->left),maxDepth(root->right));
    }
};
```

## 1. Problem Explanation

The problem is to find the maximum depth of a binary tree, which is the number of nodes along the longest path from the root node to any leaf node. A leaf node is a node that has no left or right children. If the tree is empty, its maximum depth is `0`. Otherwise, the depth is calculated by considering the depths of the left and right subtrees and selecting the larger one.

## 2. Core Idea

The solution uses recursion to calculate the maximum depth of the binary tree. The base case checks whether the current node is `nullptr`; if it is, the function returns `0` because an empty subtree has no nodes. For a non-empty node, the function recursively calculates the maximum depth of its left subtree using `maxDepth(root->left)` and its right subtree using `maxDepth(root->right)`. It then takes the larger of these two depths and adds `1` to include the current node. This process continues until the maximum depth of the entire tree is calculated.

## 3. Implementation Explanation

The `maxDepth(TreeNode* root)` function accepts a pointer to the root node and returns an integer representing the tree's maximum depth. First, `if (!root)` handles the base case by returning `0` when the current node is null. Next, `maxDepth(root->left)` and `maxDepth(root->right)` recursively calculate the depths of the left and right subtrees. The `max()` function selects the greater depth, and adding `1` accounts for the current node. Finally, the calculated value is returned to the previous recursive call until the result reaches the original function call.

Time Complexity: `O(n)` — Each node is visited exactly once, where nnn is the number of nodes in the tree.

Space Complexity: `O(h)` — The recursive call stack requires space proportional to the tree's height hhh. This is `O(log⁡n)` for a balanced tree and `O(n)` for a completely skewed tree.

Key takeaway: The maximum depth of a binary tree is the greater of its left and right subtree depths, plus one for the current node.
