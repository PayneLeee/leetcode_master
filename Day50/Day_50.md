# 随想录day 50 | 198. 打家劫舍 213. 打家劫舍 II 337. 打家劫舍III

## 198. 打家劫舍
[题目链接](https://leetcode.cn/problems/house-robber/description/)
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int size = nums.size();
        vector<int> table(size);
        if (size == 0) return 0;
        if (size == 1) return nums[0];
        if (size == 2) return max(nums[0],nums[1]);
        table[0] = nums[0];
        table[1] = max(nums[0],nums[1]);
       // int ans = max(nums[0],nums[1]) ;
        for (int i = 2; i < size; i++) {
            table[i] = max(table[i-1], table[i-2] + nums[i]);
        }
        return table[size-1];
    }
};
```

## 213 打家劫舍 II 

[题目链接 213](https://leetcode.cn/problems/house-robber-ii/description/)
你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

```cpp
class Solution {
public:
    int robInSequence(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        if (nums.size() == 2) return max(nums[0], nums[1]);
        vector<int> dp (nums.size(), 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for(int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i-1]);
        }
        return dp[nums.size() -1];
    }
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        if (nums.size() == 2) return max(nums[0], nums[1]);
        vector<int> left(nums.begin(), nums.end() - 1);
        vector<int> right(nums.begin() + 1, nums.end());
        return max(robInSequence(left), robInSequence(right));
    }
};
```
## 337. 打家劫舍III

[题目链接 337](https://leetcode.cn/problems/house-robber-iii/description/)
小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 root 。

除了 root 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 两个直接相连的房子在同一天晚上被打劫 ，房屋将自动报警。

给定二叉树的 root 。返回 在不触动警报的情况下 ，小偷能够盗取的最高金额 。
### 第一次提交
直接遍历，没有记忆化搜索，用的结构访问也很慢
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
    vector<int> traversal(TreeNode* node) {
        vector<int> res(2);
        if (node->left == 0 && node->right == 0) {
            res[0] = 0;
            res[1] = node->val;
            return res;
        } else if (node->left == 0 || node->right == 0) {
            if (node->left != 0) {
                res[0] = max(traversal(node->left)[0], traversal(node->left)[1]);
                res[1] = node->val + traversal(node->left)[0];
                return res;
            } else {
                res[0] = max(traversal(node->right)[0], traversal(node->right)[1]);
                res[1] = node->val + traversal(node->right)[0];
                return res;
            }            
        } else {
            vector<int> left = traversal(node->left);
            vector<int> right = traversal(node->right);
            res[0] = max(left[0], left[1]) + max(right[0], right[1]);
            res[1] = node->val + left[0] + right[0];
            return res;
        }
    }
    int rob(TreeNode* root) {
        vector<int> res = traversal (root);
        return max(res[0], res[1]);
    }
};
```
### 学习题解
记忆化搜索，注意unordered_map的key可以是指针，效率也很高
```cpp
class Solution {
public:
    unordered_map<TreeNode* , int> umap; // 记录计算过的结果
    int rob(TreeNode* root) {
        if (root == NULL) return 0;
        if (root->left == NULL && root->right == NULL) return root->val;
        if (umap[root]) return umap[root]; // 如果umap里已经有记录则直接返回
        // 偷父节点
        int val1 = root->val;
        if (root->left) val1 += rob(root->left->left) + rob(root->left->right); // 跳过root->left
        if (root->right) val1 += rob(root->right->left) + rob(root->right->right); // 跳过root->right
        // 不偷父节点
        int val2 = rob(root->left) + rob(root->right); // 考虑root的左右孩子
        umap[root] = max(val1, val2); // umap记录一下结果
        return max(val1, val2);
    }
};
```

其实我的做法用了一个树形dp， 这里整理一下代码，实践证明vector<int>的创建很费时间
初始化的时候可以直接对空节点初始化
```cpp
class Solution {
public:
    vector<int> traversal(TreeNode* node) {
        //vector<int> res(2);
        if (!node) {
            return vector<int>{0, 0};
        } else {
            vector<int> left = traversal(node->left);
            vector<int> right = traversal(node->right);
            int no = max(left[0], left[1]) + max(right[0], right[1]);
            int yes = node->val + left[0] + right[0];
            return vector<int>{no, yes};
        }
    }
    int rob(TreeNode* root) {
        vector<int> res = traversal (root);
        return max(res[0], res[1]);
    }
};
```

运用结构体可以进一步加速
```cpp
struct SubtreeStatus {
    int selected;
    int notSelected;
};

class Solution {
public:
    SubtreeStatus dfs(TreeNode* node) {
        if (!node) {
            return {0, 0};
        }
        auto l = dfs(node->left);
        auto r = dfs(node->right);
        int selected = node->val + l.notSelected + r.notSelected;
        int notSelected = max(l.selected, l.notSelected) + max(r.selected, r.notSelected);
        return {selected, notSelected};
    }

    int rob(TreeNode* root) {
        auto rootStatus = dfs(root);
        return max(rootStatus.selected, rootStatus.notSelected);
    }
};

```