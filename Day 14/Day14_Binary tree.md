# 2024/4/24 代码随想录Day 14： 二叉树 | 二叉树深度优先遍历 | lc144 二叉树的前序遍历 | lc145 二叉树的后序遍历 | lc94 二叉树的中序遍历

## 二叉树理论基础
[随想录](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)


## 二叉树的前中后序遍历
[前序144](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/) 给你二叉树的根节点 root ，返回它节点值的 前序 遍历。

[后序145](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)给定一个二叉树的根节点 root ，返回 它的 后序 遍历 

[中序94](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)给定一个二叉树的根节点 root ，返回 它的 中序 遍历 

## 递归方法
1. 确定递归函数的参数和返回值： 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

2. 确定终止条件： 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

3. 确定单层递归的逻辑： 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。
   
PreOrder
```cpp
class Solution {
public:
    void preOrder(TreeNode* node, vector<int>& res) {
        if (node == nullptr) return;
        res.push_back(node->val);
        preOrder(node->left, res);
        preOrder(node->right, res);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        preOrder(root, res);
        return res;
        
    }
};
```
PostOrder
```cpp
class Solution {
public:
    void postOrder(TreeNode* node, vector<int>& res) {
        if (node == nullptr) return;        
        postOrder(node->left, res);
        postOrder(node->right, res);
        res.push_back(node->val);
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        postOrder(root, res);
        return res;
        
    }
};
```
Inorder
```
class Solution {
public:
    void inOrder(TreeNode* root, vector<int>& res) {
        if (root == nullptr)
            return;
        inOrder(root->left, res);
        res.push_back(root->val);
        inOrder(root->right, res);
    };
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        inOrder(root, res);
        return res;
    }
};
```
## 迭代遍历
[随想录](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%BF%AD%E4%BB%A3%E9%81%8D%E5%8E%86.html#%E6%80%9D%E8%B7%AF)
Preorder
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> res;
        if (root == nullptr) return res;
        st.push(root);
        while(!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();
            res.push_back(cur->val);
            if (cur->right) st.push(cur->right);
            if (cur->left) st.push(cur->left);            
        }        
        return res;
    }
};
```
PostOrder
![alt text](image.png)
```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        if (root == nullptr) return res;
        st.push(root);
        while(!st.empty()) {
            TreeNode* temp = st.top();
            st.pop();
            res.push_back(temp->val);
            if (temp->left) st.push(temp->left);
            if (temp->right) st.push(temp->right);
        }
        reverse(res.begin(), res.end());
        return res;
        
    }
```
Inorder
写出来并不是那么顺畅

复杂原因：访问和处理的顺序不一致

处理：将元素放进result数组中

访问：遍历节点
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        if (root == nullptr) return res;
        TreeNode* cur = root;
        while(!st.empty() || cur != nullptr) {
            if (cur != nullptr) {
                st.push(cur);
                cur = cur->left;
            } else {
                cur = st.top();
                st.pop();
                res.push_back(cur->val);
                cur = cur->right;
            }
        }
        return res;
    }
};
```
## 统一迭代遍历
[随想录](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%BB%9F%E4%B8%80%E8%BF%AD%E4%BB%A3%E6%B3%95.html#%E6%80%9D%E8%B7%AF)

核心思路：用null来标志处理的次序
preOrder
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> res;
        if (root == nullptr) return res;
        st.push(root);
        while(!st.empty()) {
            TreeNode* cur = st.top();
            if (cur == nullptr) {
                st.pop();
                cur = st.top();
                //处理入列
                res.push_back(cur->val);
                st.pop();
            } else {
                st.pop();
                if (cur->right) st.push(cur->right);
                if (cur->left) st.push(cur->left);
                st.push(cur);
                st.push(nullptr);                
            }
        }        
        return res;
    }
};
```
PostOrder
注意不能在其他情况让nullptr入栈
```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        if (root == nullptr) return res;
        st.push(root);
        while(!st.empty()) {
            TreeNode* cur= st.top();
            if (cur == nullptr) {
                st.pop();
                cur = st.top();
                res.push_back(cur->val);
                st.pop();
            } else {
                st.pop();
                st.push(cur);
                st.push(nullptr);
                if (cur->right) st.push(cur->right);
                if (cur->left) st.push(cur->left);
            }   
        }   
        return res;  
    }
};
```
InOrder
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        if (root == nullptr) return res;
        st.push(root);
        while(!st.empty()) {
            TreeNode* cur = st.top();
            if (cur == nullptr) {
                st.pop();
                cur = st.top();
                res.push_back(cur->val);
                st.pop();
            } else {
                st.pop();
                if (cur -> right) st.push(cur->right);
                st.push(cur);
                st.push(nullptr);
                if (cur -> left) st.push(cur->left);
            };
        }
        return res;
    }
};
```