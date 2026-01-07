# 代码随想录 Day23| 530.二叉搜索树的最小绝对差  501.二叉搜索树中的众数  236. 二叉树的最近公共祖先  

## 530.二叉搜索树的最小绝对差 

[题目链接 530](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)
给你一个二叉搜索树的根节点 root ，返回 树中任意两不同节点值之间的最小差值 。

差值是一个正数，其数值等于两值之差的绝对值。
### 第一次提交
很自然的直接中序遍历

```cpp
class Solution {
public:
    TreeNode* pre = nullptr;
    int res = INT_MAX;
    void dfs(TreeNode* root) {
        if (root == nullptr) return;
        dfs(root -> left);
        if (pre) {
            res = min(res, root->val - pre->val);
        }
        pre = root;
        dfs(root -> right);
    }
    int getMinimumDifference(TreeNode* root) {
        dfs(root);
        return res;
    }
};
```
[随想录](https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html#%E6%80%9D%E8%B7%AF)
和我的思路基本完全一样

## 501.二叉搜索树中的众数
[题目链接 501](https://leetcode.cn/problems/find-mode-in-binary-search-tree/description/)

### 第一次提交
非常不优雅的直接用哈希表解决，实际上解决了更复杂的问题，任意一个树的众数

注意这里遍历map的时候是 it != end(),iterator 之间没有定义大小关系

注意这里class需要public不然不能直接访问构造函数
```cpp
class Solution {
public:
    unordered_map<int, int> mapNodeToCount;
    void dfs(TreeNode* root) {
        if(root == nullptr)return;
        dfs(root ->left);
        if (mapNodeToCount.count(root->val)) mapNodeToCount[root->val]++;
        else mapNodeToCount[root->val] = 1;
        dfs(root->right);
    }
    class cmp {
        public:
        cmp(){}
        bool operator() (const std::pair<int, int> &left, const std::pair<int, int> &right) {
            return left.second > right.second;
        }    
    };
    vector<int> findMode(TreeNode* root) {
        dfs(root);
        vector< pair<int, int> > container;
        for (unordered_map<int, int> :: iterator it = mapNodeToCount.begin(); it != mapNodeToCount.end(); it++) {
            container.push_back(*it);
        }
        sort(container.begin(), container.end(), cmp());
        int max = container[0].second;
        vector<int> res;
        for (int i = 0; i < container.size(); i++) {
            if (container[i].second == max) res.push_back(container[i].first);
            if (container[i].second < max) break; 
        }
        return res;
    }
};
```
### 学习优秀题解
[视频链接](https://www.bilibili.com/video/BV1fD4y117gp/)
双指针法，maintain一个count 一个maxcount，遍历一遍
#### 按双指针思路
```cpp
class Solution {
public:
    TreeNode* pre = nullptr;
    int count, maxCount;
    vector<int> res;
    void traversal(TreeNode* node) {
        if (node == nullptr) return;
        traversal(node->left);
        if (pre == nullptr) {
            count = 1;
            maxCount = 1;
        }
        else if (pre->val != node->val) {
            count = 1;
        } else if (pre->val == node-> val) {
            count ++;
        }
        if (count == maxCount) res.push_back(node->val);
        else if (count > maxCount) {
                maxCount = count;
                res.clear();
                res.push_back(node->val);
            }
        pre = node;
        traversal(node->right);
    }
    vector<int> findMode(TreeNode* root) {
        traversal(root);
        return res;
    }
};
```
### 对cmp结构生疏了，回顾自定义比较函数- Sort & PriorityQueue 
#### Sort
[Sort function DOC](https://en.cppreference.com/w/cpp/algorithm/sort)

[Youtube](https://www.youtube.com/watch?v=lz2w1t5twtU)
![请添加图片描述](https://img-blog.csdnimg.cn/direct/0693ff47890646638ead061bb55febc5.png)

```cpp
struct
{
    bool operator()(int a, int b) const { return a < b; }
}
customLess;

bool less_by_second_char(const std::string &s1, const std::sztring &s2) {
    return s1.substr(1) < s2.sustr(1);
}

class str_comparator {
    public:
    str_comparator(int i = 0) : offset(i) {}
    bool operator() (const std::string &s1, const std::sztring &s2) {
        return s1.substr(offset) < s2.sustr(offset);
        }
    private:
        int offset;
    
}
```

return left>right : 大到小

#### PriorityQueue
初始化: 
```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pri_que;
```
![请添加图片描述](https://img-blog.csdnimg.cn/direct/0be7f137091b47ce81dc39764bb934f3.png)

[priority queue DOC](https://en.cppreference.com/w/cpp/container/priority_queue)
![请添加图片描述](https://img-blog.csdnimg.cn/direct/61722b0aa8324598aaede6ac5e0dfb21.png)

[compare type DOC](https://en.cppreference.com/w/cpp/named_req/Compare)
```cpp
// Using a custom function object to compare elements.
struct
{
    bool operator()(const int l, const int r) const { return l > r; }
} customLess;
```

return left>right : 小顶堆
### 访问iterator
```cpp
 for (unordered_map<int, int>::iterator it = map.begin(); it != map.end(); it++) {
}
```

## 236. 二叉树的最近公共祖先

[题目链接 236](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

### 第一次提交
思路是后序遍历，找到第一个既能找到p，也能找到q的，用两个bool来表示是不是p或者q的祖先,一个flag 表示是否找到，合理剪枝。

```cpp
class Solution {
public:
    TreeNode* result = nullptr;
    bool flag = false;
    pair<bool, bool> traversal(TreeNode* node,TreeNode* p,TreeNode* q) {
        pair<bool, bool> res{false, false};
        if (node == nullptr) return res;
        if (flag) return res;
        pair<bool, bool> left = traversal(node->left, p, q);
        pair<bool, bool> right = traversal(node->right, p, q);
        res.first = node->val == p->val || left.first || right.first;
        res.second = node->val == q->val || left.second || right.second;
        if (res.first == true && res.second == true && !flag) {
            result = node;
            flag = true;
        }
        return res;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        traversal(root, p, q);
        return flag? result : root;
    }
};
```
### 学习优秀题解
[随想录](https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#%E6%80%9D%E8%B7%AF)
可以直接用函数本身进行回溯，没找到就是nullptr；
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr || root == p || root == q) return root;
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p, q);
        if (left != nullptr && right != nullptr) return root;
        else if (left == nullptr && right != nullptr) return right;
        else if (left != nullptr && right == nullptr) return left;
        else return nullptr;
    }
};
```