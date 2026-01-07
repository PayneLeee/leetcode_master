# 代码随想录算法训练营第21天 | 513.找树左下角的值、112. 路径总和、113.路径总和ii、106.从中序与后序遍历序列构造二叉树、105.从前序与中序遍历序列构造二叉树

## 513.找树左下角的值

[题目链接 513](https://leetcode.cn/problems/find-bottom-left-tree-value/) 
给定一个二叉树的 根节点 root，请找出该二叉树的 最底层 最左边 节点的值。

假设二叉树中至少有一个节点。
[随想录](https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

### 第一次提交
直接想到层序遍历，轻松AC
```cpp
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        if (!root) return -1;
        int res = root -> val; 
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()) {
            res = que.front()->val;
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
        }
        return res;       
    }
};
```
### 学习题解
递归方法
深度遍历
[视频链接](https://www.bilibili.com/video/BV1424y1Z7pn/)
```cpp
class Solution {
public:
    int maxDepth = 0;
    int res = -1;
    void traversal (TreeNode* node, int depth) {
        if (node->left == nullptr && node->right == nullptr && depth > maxDepth) {
            maxDepth = depth;
            res = node->val;
        }
        if (node->left) traversal(node->left, depth + 1);//这里隐藏了对depth 的回溯
        if (node->right) traversal(node->right, depth + 1);
    }
    int findBottomLeftValue(TreeNode* root) {
        res = root->val;
        traversal(root, 0);
        return res;           
    }
};
```
## 112. 路径总和
[题目链接 112](https://leetcode.cn/problems/path-sum/description/) 

### 第一次提交
简单迭代
```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root) return false;
        if (root->left == nullptr && root->right == nullptr && targetSum == root->val) return true;
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
    }
};
```
### 学习优秀题解
[视频链接](https://www.bilibili.com/video/BV19t4y1L7CR/?vd_source=4cca6f0dd2495280b5f065c0e86f221c)
深度遍历回溯方法

多一种终止条件
```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root) return false;
        if (root->left == nullptr && root->right == nullptr && targetSum == root->val) return true;
        //这种情况下也可以剪枝
        if (root->left == nullptr && root->right == nullptr && targetSum != root->val) return false;
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
    }
};
```
## 113.路径总和ii
[题目链接](https://leetcode.cn/problems/path-sum-ii/description/) 给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。
叶子节点 是指没有子节点的节点。

很自然的想到回溯
```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void traversal(TreeNode* node, int targetSum) {
        if (node == nullptr) return;
        if (node->left == nullptr && node->right == nullptr && targetSum == 0) {
            res.push_back(path);
            return;
        }
        if (node->left == nullptr && node->right == nullptr && targetSum != 0) {
            return;
        }
        if(node->left) {
            path.push_back(node->left->val);
            traversal(node->left, targetSum - node->left->val);
            path.pop_back();
        }
        if(node->right){
            path.push_back(node->right->val);
            traversal(node->right, targetSum - node->right->val);
            path.pop_back();
        }
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if(!root) return res;
        path.push_back(root->val);
        traversal(root, targetSum - root->val);
        return res;
    }
};
```

[随想录](https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html#%E7%9B%B8%E5%85%B3%E9%A2%98%E7%9B%AE%E6%8E%A8%E8%8D%90) 
整体思路变化不大
