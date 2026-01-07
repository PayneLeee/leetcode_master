# 代码随想录Day24 |  235. 二叉搜索树的最近公共祖先  701.二叉搜索树中的插入操作  450.删除二叉搜索树中的节点 


##  235. 二叉搜索树的最近公共祖先

[题目链接 235](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/description/) 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

假设二叉树中至少有一个节点。

[随想录](https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)

### 第一次提交
利用搜索树性质，

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root ==  nullptr) return nullptr;
        //cout<< root->val<<endl;
        int minVal = min(p->val, q->val);
        int maxVal = max(p->val, q->val);
        if (minVal <= root->val && root->val <= maxVal) return root;
        if (root->val < minVal) return lowestCommonAncestor(root->right,p,q);
        if (root->val > maxVal) return lowestCommonAncestor(root->left,p,q);
        return nullptr;     
    }
};
```
## 701.二叉搜索树中的插入操作

[题目链接 701](https://leetcode.cn/problems/insert-into-a-binary-search-tree/description/)给定二叉搜索树（BST）的根节点 root 和要插入树中的值 value ，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。

### 第一次提交
找到叶子节点，其前一个结点就是插入节点的父亲节点
```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode* insert = new TreeNode(val);
        if (root == nullptr) return insert;
        TreeNode* cur = root;
        TreeNode* pre = nullptr;        
        while(cur!= nullptr) {
            pre = cur;
            if (cur->val < val) cur = cur->right;
            else cur = cur->left;
        }
        if (pre->val < val) pre->right = insert;
        else pre->left = insert;
        return root;
    }
};
```
### 学习优秀题解

[随想录](https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE) 
第一次写直接用双指针写出了递归法，这里写一下迭代方法，遇到的第一个空节点换成需要插入的节点即可

递归看着更简洁
```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode* insert = new TreeNode(val);
        if (root == nullptr) return insert;
        if (root->val < val) root->right = insertIntoBST(root->right, val);
        if (root->val > val) root->left = insertIntoBST(root->left, val);
        return root;
    }
};
```

## 450.删除二叉搜索树中的节点 
[题目链接 450](https://leetcode.cn/problems/delete-node-in-a-bst/description/) 给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

首先找到需要删除的节点；
如果找到了，删除它。

### 第一次提交
分为找到和插入两部分, 加入了dummyhead 来应对删除root节点的情况。 查找的时候需要记录前一位的节点以及在节点左边还是右边。

```cpp
class Solution {
public:
    TreeNode*pre = nullptr;
    string flag = "";
    TreeNode* searchKey(TreeNode* node, int key) {
        if (node == nullptr) return nullptr;
        if (node->val == key) return node;
        if (node->val > key) {
            pre = node;
            flag = "left";
            return searchKey(node->left, key);
        }
        if (node->val < key) {
            pre = node;
            flag = "right";
            return searchKey(node->right, key);
        }
        return nullptr;
    }
    TreeNode* deleteNode(TreeNode* root, int key) {
        TreeNode* dummyHead =  new TreeNode(0, root, nullptr);
        pre = dummyHead;
        flag = "left";
        TreeNode* target = searchKey(root, key);
        if (target == nullptr) return root;
        // cout << pre->val << " PPreval " << flag << " FFlag " <<endl; check pre and indicator
        // return nullptr;

        //Delete target
        //Notice that delete root is a special case
        if (target->left == nullptr && target->right == nullptr) {
            if (flag == "left") {
                pre -> left = nullptr;
            } else {
                pre -> right = nullptr;
            }
        } else if (target->left != nullptr && target->right == nullptr){
            if (flag == "left") {
                pre -> left = target->left;
            } else {
                pre->right = target->left;
            }
        } else if (target->left == nullptr && target->right != nullptr) {
            if (flag == "left") {
                pre -> left = target->right;
            } else {
                pre -> right = target->right;
            }
        } else if (target->left != nullptr && target ->right != nullptr) {
            TreeNode* leftRoot = target->left;
            TreeNode* rightMin = target->right;
            while(rightMin->left != nullptr) rightMin = rightMin->left;
            rightMin->left = leftRoot;
            if (flag == "left") {
                pre -> left = target->right;
            } else {
                pre -> right = target->right;
            }
        }
        return dummyHead->left;
    }
};
```
### 学习优秀题解
[随想录](https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)： 迭代方法可以遍历的同删除

这里就把二叉搜索树中删除节点遇到的情况都搞清楚。

有以下五种情况：

第一种情况：没找到删除的节点，遍历到空节点直接返回了
找到删除的节点

第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点

第三种情况：删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点

第四种情况：删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点

第五种情况：左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点。

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
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) return root;
        if (root->val > key) root->left = deleteNode(root->left, key);
        if (root->val < key) root->right = deleteNode(root->right, key);
        if (root->val == key) {
            // 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
            if (root->left == nullptr && root->right == nullptr) {
                ///! 内存释放
                delete root;
                return nullptr;
            }
            // 第三种情况：其左孩子为空，右孩子不为空，删除节点，右孩子补位 ，返回右孩子为根节点
            else if (root->left == nullptr) {
                auto retNode = root->right;
                ///! 内存释放
                delete root;
                return retNode;
            }
            // 第四种情况：其右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
            else if (root->right == nullptr) {
                auto retNode = root->left;
                ///! 内存释放
                delete root;
                return retNode;
            }
            // 第五种情况：左右孩子节点都不为空，则将删除节点的左子树放到删除节点的右子树的最左面节点的左孩子的位置
            // 并返回删除节点右孩子为新的根节点。
            else {
                TreeNode* cur = root->right; // 找右子树最左面的节点
                while(cur->left != nullptr) {
                    cur = cur->left;
                }
                cur->left = root->left; // 把要删除的节点（root）左子树放在cur的左孩子的位置
                TreeNode* tmp = root;   // 把root节点保存一下，下面来删除
                root = root->right;     // 返回旧root的右孩子作为新root
                delete tmp;             // 释放节点内存（这里不写也可以，但C++最好手动释放一下吧）
                return root;
            }
        }
        
        return root;
    }
};
```