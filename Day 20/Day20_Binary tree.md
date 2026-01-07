# 2024/4/28 代码随想录Day 20： 110.平衡二叉树  257. 二叉树的所有路径  404.左叶子之和

## 110.平衡二叉树

[题目链接 110](110.平衡二叉树) 给定一个二叉树，判断它是否是 
平衡二叉树。

 
[随想录](https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html)
### 第一次提交
很自然的会想到计算深度然后判断

条件为左边也是，右边也是 再加上两边深度差距不大。
```cpp
class Solution {
public:
    int depth (TreeNode* node) {
        if(node == nullptr) return 0;
        return max(depth(node->left), depth(node->right)) +1;
    }
    bool isBalanced(TreeNode* root) {
        if (root == nullptr) return true;
        if(isBalanced(root->left) && isBalanced(root->right)) {
            if(abs(depth(root->left) - depth(root->right)) <= 1) return true;
        }
        return false;
    }
};
```
### 学习优秀题解
[视频](https://www.bilibili.com/video/BV1Ug411S7my/?vd_source=4cca6f0dd2495280b5f065c0e86f221c) 思路相近

## 257. 二叉树的所有路径

[111二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/description/)
 给你一个二叉树的根节点 root ，按 任意顺序 ，返回所有从根节点到叶子节点的路径。

叶子节点 是指没有子节点的节点。

### 第一次提交
左右头的遍历后序递归方式

```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        if (!root) return res;
        if (root->left == nullptr && root->right == nullptr) {
            res.push_back(to_string(root->val));
            return res;
        }
        vector<string> left = binaryTreePaths(root->left);
        vector<string> right = binaryTreePaths(root->right);
        for (string s : left) {
            res.push_back(to_string(root->val) + "->" + s);
        }
        for (string r : right) {
            res.push_back(to_string(root->val) + "->" + r);
        }
        return res;
    }
};
```
### 学习优秀题解
回溯方法，随想录中给的前序方法，很有回溯的感觉

这里我的办法感觉更加简洁，但是随想录中展现的回溯思想更有普适性
#### 展现回溯的深度遍历
很顺利一遍过
```cpp
class Solution {
public:
    vector<int> nodeNumbers;
    vector<string> res;
    string intVectorToString (vector<int> vec) {
        string res;
        for (int i : vec) {
            res = res + to_string(i) + "->";
        }
        res.pop_back();
        res.pop_back();
        return res;
    }
    void traversal (TreeNode* node){
        nodeNumbers.push_back(node->val);
        if (node->left == nullptr && node->right == nullptr) {
            res.push_back(intVectorToString(nodeNumbers));
            return;
        }
        if (node->left) {
            traversal(node->left);
            nodeNumbers.pop_back();
        }
        if (node->right) {
            traversal(node->right);
            nodeNumbers.pop_back();
        }
    }
    vector<string> binaryTreePaths (TreeNode* root) {
        traversal (root);
        return res;
    }
};
```
#### 隐藏回溯过程的深度遍历
```cpp
class Solution {
private:

    void traversal(TreeNode* cur, string path, vector<string>& result) {
        path += to_string(cur->val); // 中
        if (cur->left == NULL && cur->right == NULL) {
            result.push_back(path);
            return;
        }
        if (cur->left) traversal(cur->left, path + "->", result); // 左
        if (cur->right) traversal(cur->right, path + "->", result); // 右
    }

public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        string path;
        if (root == NULL) return result;
        traversal(root, path, result);
        return result;

    }
};
```

## 404.左叶子之和

[题目链接 404](https://leetcode.cn/problems/sum-of-left-leaves/description/)
给定二叉树的根节点 root ，返回所有左叶子之和。

### 第一次提交
直接分类讨论递归通过

```cpp
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if (root == nullptr) return 0;
        if (root->right == nullptr && root->left == nullptr ) {
            return 0;
        }
        if (root->right == nullptr && root->left != nullptr ) {
            if (root->left->left == nullptr && root->left->right == nullptr) return root->left->val;
            else return sumOfLeftLeaves(root->left);
        }
        if ( root->left == nullptr  && root->right != nullptr) {
            return sumOfLeftLeaves(root->right);
        }
        if (root->right != nullptr && root->left != nullptr ) {
            if (root->left->left == nullptr && root->left->right == nullptr) return root->left->val + sumOfLeftLeaves(root->right);
            else return sumOfLeftLeaves(root->left) + sumOfLeftLeaves(root->right);
        }
        return 0;
    }
};
```
### 学习题解
两种经典方法 [深度遍历和广度遍历](https://leetcode.cn/problems/sum-of-left-leaves/solutions/419103/zuo-xie-zi-zhi-he-by-leetcode-solution/)
#### 深度遍历
实际上和我的做法没有区别

```cpp
class Solution {
public:
    bool isLeafNode(TreeNode* node) {
        if (node != nullptr && node->left == nullptr && node->right == nullptr) {
            return true;
        }
        return false;
    }
    int dfs (TreeNode* node) {
        int ans = 0;
        if (node->left) {
            ans += isLeafNode(node->left) ? node->left->val : dfs(node->left);
        }
        if (node->right && !isLeafNode(node->right)) {
            ans += dfs(node->right);
        }
        return ans;
    }
    int sumOfLeftLeaves(TreeNode* root) {
        return root? dfs(root) : 0;
    }
};
```
可以直接用原来的函数体
```cpp
class Solution {
public:
    bool isLeafNode(TreeNode* node) {
        if (node != nullptr && node->left == nullptr && node->right == nullptr) {
            return true;
        }
        return false;
    }

    int sumOfLeftLeaves(TreeNode* root) {
        if (!root) return 0;
        int ans = 0;
        if (root->left) {
            ans += isLeafNode(root->left) ? root->left->val : sumOfLeftLeaves(root->left);
        }
        if (root->right && !isLeafNode(root->right)) {
            ans += sumOfLeftLeaves(root->right);
        }
        return ans;
    }
};
```
#### 广度
```cpp
class Solution {
public:
    bool isLeafNode(TreeNode* node) {
        return !node->left && !node->right;
    }

    int sumOfLeftLeaves(TreeNode* root) {
        if (!root) {
            return 0;
        }

        queue<TreeNode*> q;
        q.push(root);
        int ans = 0;
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            if (node->left) {
                if (isLeafNode(node->left)) {
                    ans += node->left->val;
                }
                else {
                    q.push(node->left);
                }
            }
            if (node->right) {
                if (!isLeafNode(node->right)) {
                    q.push(node->right);
                }
            }
        }
        return ans;
    }
};
```