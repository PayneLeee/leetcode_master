# 2024/4/28 代码随想录Day 18： 二叉树 |102 层序遍历、226翻转二叉树、101对称二叉树

## 102 二叉树的层序遍历
[随想录](https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

[视频链接](https://www.bilibili.com/video/BV1GY4y1u7b2/?vd_source=4cca6f0dd2495280b5f065c0e86f221c)

写的比较多了，多看看怎么精简一下写法
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<int> level;
        vector<vector<int>> res;
        queue<TreeNode*> que;
        if (!root) return res;
        que.push(root);
        while(!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                level.push_back(cur->val);
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
            }
            res.push_back(level);
            level.clear();
        }
        return res;
    }
};
```
写没怎么写过的递归写法，也不是太好想，按层递归
```cpp
class Solution {
public:
    void level(TreeNode* node, vector<vector<int>>& res, int depth) {
        if (!node)
            return; // stop
        if (res.size() == depth)
            res.push_back(vector<int>());
        res[depth].push_back(node->val);
        if (node->left)
            level(node->left, res, depth + 1);
        if (node->right)
            level(node->right, res, depth + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        int depth = 0;
        vector<vector<int>> res;
        level(root, res, depth);
        return res;
    }
};
```
## 226. 翻转二叉树

[题目链接](https://leetcode.cn/problems/invert-binary-tree/)
1. 确定递归函数的参数和返回值

2. 确定终止条件

3. 确定单层递归的逻辑
PreOrder
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) return root;
        swap(root->left, root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```
PostOrder
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) return root;
        invertTree(root->left);
        invertTree(root->right);
        swap(root->left, root->right);
        return root;
    }
};
```
InOrder
注意这里交换后left和right交换了
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) return root;
        invertTree(root->left);
        swap(root->left, root->right);
        invertTree(root->left);
        return root;
    }
};
```

复习一下递归写法：
PreOrder：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return root;
        stack<TreeNode*> st;
        st.push(root);
        while(!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();
            if (cur->left) st.push(cur->left);
            if (cur->right) st.push(cur->right);
            swap(cur->left, cur->right);
        }
        return root;
    }
};
```
Uniform PreOrder：
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return root;
        stack<TreeNode*> st;
        st.push(root);
        while(!st.empty()) {
            TreeNode* cur = st.top();
            if (cur == nullptr) {
                st.pop();
                cur = st.top();
                swap(cur->left, cur->right);
                st.pop();
            } else {
                st.pop();
                if (cur->right) st.push(cur->right);
                if (cur->left) st.push(cur->left);
                st.push(cur);
                st.push(nullptr);
            }
        }
        return root;
    }
};
```

## 101.对称二叉树
[题目链接 101](https://leetcode.cn/problems/symmetric-tree/description/)
给你一个二叉树的根节点 root ， 检查它是否轴对称。
### 第一次提交
第一想法是递归，但是时间效率很低
```cpp
class Solution {
public:
    bool isInverse(TreeNode* leftNode, TreeNode* rightNode) {
        if (leftNode == nullptr && rightNode == nullptr) return true;
        if (leftNode == nullptr && rightNode != nullptr) return false;
        if (rightNode == nullptr && leftNode != nullptr) return false;
        if (rightNode->val != leftNode->val) return false;
        return (isInverse(leftNode->left, rightNode->right) && isInverse(leftNode->right, rightNode->left));
    }
    bool isSymmetric(TreeNode* root) {
        if (root == nullptr) return false;
        return isInverse(root-> left, root-> right);
    }
};
```
