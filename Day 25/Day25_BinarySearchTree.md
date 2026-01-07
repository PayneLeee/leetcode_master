# 代码随想录Day25 | 669. 修剪二叉搜索树  108.将有序数组转换为二叉搜索树 538.把二叉搜索树转换为累加树 

##  669. 修剪二叉搜索树

[题目链接 669](https://leetcode.cn/problems/trim-a-binary-search-tree/) 给你二叉搜索树的根节点 root ，同时给定最小边界low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在[low, high]中。修剪树 不应该 改变保留在树中的元素的相对结构 (即，如果没有被移除，原有的父代子代关系都应当保留)。 可以证明，存在 唯一的答案 。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

### 第一次提交
直观的迭代方法，一遍过
```cpp
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (root == nullptr) return nullptr;
        if (root-> val < low ) {
            return trimBST(root->right,low, high);
        } else if (root->val > high) {
            return trimBST(root->left, low, high);
        } else {
            root ->left = trimBST(root->left, low, high);
            root->right = trimBST(root->right, low, high);
            return root;
        }
        return root;        
    }
};
```
### 学习优秀题解
[随想录](https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E6%80%9D%E8%B7%AF) 思路基本一致
熟悉一下递归思路和方式

因为二叉搜索树的有序性，不需要使用栈模拟递归的过程。

在剪枝的时候，可以分为三步：

将root移动到[L, R] 范围内，注意是左闭右闭区间

剪枝左子树

剪枝右子树

```cpp
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        //find new root;
        if (root == nullptr) return nullptr;
        while(root != nullptr && (root->val > high || root->val < low)) {
            if (root->val > high) {
                root = root->left;
            } else {
                root = root->right;
            }
        }
        //remove all element that smaller than low
        TreeNode* cur = root;
        while(cur) {
            while ( cur->left != nullptr && cur -> left->val < low ) {
                cur->left = cur->left->right;
            }
            cur = cur->left;
        }    
        //remove all element that greater than high
        cur = root;
        while(cur) {
            while (cur->right != nullptr && cur -> right ->val > high ) {
                cur->right = cur->right->left;
            }
            cur = cur->right;
        }
        return root;
    }
};
```
##  108.将有序数组转换为二叉搜索树

[题目链接 108](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)
给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 
平衡
 二叉搜索树。

 ### 第一次提交
 直接迭代，有些像中序前序建树

 ```cpp
 class Solution {
public:
    TreeNode* traversal(vector<int>& nums, int start, int end) {
        if (start == end) return nullptr;
        if (start + 1 == end) {
            TreeNode* res = new TreeNode(nums[start]);
            return res;
        }
        int id = (start + end) / 2;
        TreeNode* res = new TreeNode(nums[id]);
        res -> left = traversal(nums, start, id);
        res -> right = traversal(nums, id + 1, end);
        return res;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int n = nums.size();
        return traversal(nums, 0, nums.size());
    }
};
```
[随想录](https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E6%80%9D%E8%B7%AF) 思路基本上没区别

## 538.把二叉搜索树转换为累加树 

[题目链接 538](https://leetcode.cn/problems/convert-bst-to-greater-tree/description/)
给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。

节点的右子树仅包含键 大于 节点键的节点。

左右子树也必须是二叉搜索树。

注意：本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同

### 第一次提交
很自然直接想到右中左，从后到前遍历累加就行了，需要额外记录的就是前一个值
```cpp
class Solution {
public:
    TreeNode* pre = nullptr;
    void traversal (TreeNode* root) {
        if (root == nullptr) return;
        traversal(root -> right);
        if (pre) {
            root->val += pre->val;
        }
        pre = root;
        traversal(root ->left);
    }
    TreeNode* convertBST(TreeNode* root) {
        traversal(root);
        return root;
    }
};
```
### 学习题解
[随想录](https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html#%E6%80%9D%E8%B7%AF)
复习一下中序遍历的递归写法和统一递归写法。

#### 中序递归写法
```cpp
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        int pre = 0;
        stack <TreeNode*> st;
        if (root == nullptr) return root;
        TreeNode* cur = root;
        //st.push(root);
        while(!st.empty() || cur != nullptr) {
            if (cur != nullptr) {
                st.push(cur);
                //right                
                cur = cur -> right;
            } else {
                //center
                cur = st.top();
                st.pop();   
                cur->val += pre;
                pre = cur -> val;
                cout<< pre<< endl;
                //left
                cur = cur->left;
            }
        }
        return root;
    }
};
```

#### 统一的中序递归写法

```cpp
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        int pre = 0;
        if (root == nullptr) return nullptr;
        stack<TreeNode*> st;
        st.push(root);
        while (!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();
            if (cur == nullptr) {
                cur = st.top();
                st.pop();
                cur->val += pre;
                pre = cur->val;
            } else {
                if (cur->left) st.push(cur->left);
                st.push(cur);
                st.push(nullptr);
                if (cur->right) st.push(cur->right);
            }
        }
        return root;
    }
};
```