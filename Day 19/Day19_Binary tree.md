# 2024/4/28 代码随想录Day 19： 104二叉树的最大深度、111二叉树的最小深度、222完全二叉树的节点个数

## 104二叉树的最大深度

[题目链接](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)给定一个二叉树 root ，返回其最大深度。

二叉树的 最大深度 是指从根节点到最远叶子节点的最长路径上的节点数。

 
[随想录](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)
### 第一次提交
很自然的会想到直接递归
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        return max (maxDepth(root->left) + 1, maxDepth(root->right)+1 );
    }
};
```
### 学习优秀题解
另一种思路是广度优先搜索
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        queue<TreeNode*> que;
        if (!root) return 0;
        int res = 0;
        que.push(root);
        while(!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
            res++;
        }
        return res;
    }
};
```
## 111二叉树的最小深度
[题目链接 111](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/) 给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明：叶子节点是指没有子节点的节点。

### 第一次提交
自然也是递归比较好思考
```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        if (root->left == nullptr && root->right == nullptr) return 1;
        if (root->left != nullptr && root->right == nullptr) return minDepth(root->left) + 1;
        if (root->left == nullptr && root->right != nullptr) return minDepth(root->right) + 1;
        if (root->left != nullptr && root->right != nullptr) return min(minDepth(root->left) , minDepth(root->right)) + 1;
        return -1;
    }
};
```
### 广度遍历写法
按层去数，找到第一个左右孩子都为空的节点
```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        queue<TreeNode*> que;
        if (!root) return 0;
        que.push(root);
        int level = 0;
        while(!que.empty()) {
            level ++;
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* temp = que.front();
                que.pop();
                if (temp->left == nullptr && temp->right == nullptr) return level;
                if (temp->left) que.push(temp->left);
                if (temp->right) que.push(temp->right);
            }
        }
        return level;
    }
};
```
## 222完全二叉树的节点个数
[题目链接 222](https://leetcode.cn/problems/count-complete-tree-nodes/description/)

### 第一次提交
会直接想到递归，非常简单
```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == nullptr) {
    		return 0;
    	}
    	int left = countNodes(root->left);
    	int right = countNodes(root->right);
    	
    	return left+right+1;
    }
};
```
当然也可以广度遍历，这里再写一遍
```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        queue<TreeNode*> que;
        if (!root) return 0;
        int countNode = 0;
        que.push(root);
        while(!que.empty()) {
            TreeNode* cur = que.front();
            countNode ++;
            que.pop();
            if (cur->left) que.push(cur->left);
            if (cur->right) que.push(cur->right);
        }
        return countNode;
    }
};
```


### 学习优秀题解
这道题大概率还是更倾向于让我们利用起来二叉树的性质
[随想录](https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
[Vedio](https://www.bilibili.com/video/BV1eW4y1B7pD/?vd_source=4cca6f0dd2495280b5f065c0e86f221c)

如何判断满二叉树： 左边遍历深度和右侧遍历深度相等

**本质是剪枝**

这里注意位运算的写法
```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == nullptr) {
    		return 0;
    	}
        int leftDepth = 0;
        int rightDepth = 0;
        TreeNode* leftPath = root->left, *rightPath = root->right;
        while(leftPath != nullptr) {
            leftPath = leftPath->left;
            leftDepth++;
        }
        while(rightPath != nullptr) {
            rightPath = rightPath->right;
            rightDepth++;
        }
        if (leftDepth == rightDepth) return (2 << leftDepth) - 1;

    	int leftCnt = countNodes(root->left);
    	int rightCnt = countNodes(root->right);
    	
    	return leftCnt+rightCnt+1;
    }
};
```