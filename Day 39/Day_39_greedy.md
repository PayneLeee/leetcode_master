# 2024/5/25 Day39 greedy   738. 单调递增的数字 968. 监控二叉树


## 738. 单调递增的数字
[题目链接 738](https://leetcode.cn/problems/monotone-increasing-digits/description/)
当且仅当每个相邻位数上的数字 x 和 y 满足 x <= y 时，我们称这个整数是单调递增的。

给定一个整数 n ，返回 小于或等于 n 的最大数字，且数字呈 单调递增 。
### 第一次提交
从前往后遍历需要重复倒腾一下

```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        if (n < 10) return n;
        string number = to_string(n);
        cout<< number<< endl;  
        bool flag = true;     
        for (int i = 1; i < number.size(); i++) {
            if (flag && number[i] < number[i-1]) {
                flag = false;
                char cur = number[i-1];
                int j = i-1;
                for (; j >= 0; j--) {
                    if (number[j] == cur) number[j] -= 1;
                    else break;
                }
                i = j + 2;
            } 
            if (!flag) number[i] = '9';
        }
        return stoi(number);

    }
};
```
### 学习题解
[随想录](https://programmercarl.com/0738.%E5%8D%95%E8%B0%83%E9%80%92%E5%A2%9E%E7%9A%84%E6%95%B0%E5%AD%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
从后往前遍历可以巧妙利用比较结果
```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        if (n < 10) return n;
        string number = to_string(n);
        int flag = number.size();
        for (int i = number.size() -2; i >= 0; i--) {
            if (flag && number[i] > number[i+1]) {
                number[i] -= 1;
                flag = i;
            }
        }
        for (int i = flag + 1; i < number.size(); i++) number[i] = '9';
        return stoi(number);
    }
};
```
## 968. 监控二叉树
[题目链接 968](https://leetcode.cn/problems/binary-tree-cameras/description/)
给定一个二叉树，我们在树的节点上安装摄像头。

节点上的每个摄影头都可以监视其父对象、自身及其直接子对象。

计算监控树的所有节点所需的最小摄像头数量。
### 提交
分类讨论
```cpp
class Solution {
public:
// 0: not covered; 1: covered; 2: camera
    int result;
    int traversal(TreeNode* root) {
        if (!root) return 1;
        int left = traversal(root->left);
        int right = traversal(root->right);
        if (left == 1 && right == 1) return 0;
        else if (left == 0 || right == 0) {
            result ++;
            return 2;
        } else return 1;
    }
    int minCameraCover(TreeNode* root) {
        result = 0;
        if (traversal(root) == 0) {
            result ++;
        }
        return result;
    }
};
```
### 学习题解
[随想录](https://programmercarl.com/0968.%E7%9B%91%E6%8E%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html)
详细版本分类
```cpp
class Solution {
private:
    int result;
    int traversal(TreeNode* cur) {

        // 空节点，该节点有覆盖
        if (cur == NULL) return 2;

        int left = traversal(cur->left);    // 左
        int right = traversal(cur->right);  // 右

        // 情况1
        // 左右节点都有覆盖
        if (left == 2 && right == 2) return 0;

        // 情况2
        // left == 0 && right == 0 左右节点无覆盖
        // left == 1 && right == 0 左节点有摄像头，右节点无覆盖
        // left == 0 && right == 1 左节点有无覆盖，右节点摄像头
        // left == 0 && right == 2 左节点无覆盖，右节点覆盖
        // left == 2 && right == 0 左节点覆盖，右节点无覆盖
        if (left == 0 || right == 0) {
            result++;
            return 1;
        }

        // 情况3
        // left == 1 && right == 2 左节点有摄像头，右节点有覆盖
        // left == 2 && right == 1 左节点有覆盖，右节点有摄像头
        // left == 1 && right == 1 左右节点都有摄像头
        // 其他情况前段代码均已覆盖
        if (left == 1 || right == 1) return 2;

        // 以上代码我没有使用else，主要是为了把各个分支条件展现出来，这样代码有助于读者理解
        // 这个 return -1 逻辑不会走到这里。
        return -1;
    }

public:
    int minCameraCover(TreeNode* root) {
        result = 0;
        // 情况4
        if (traversal(root) == 0) { // root 无覆盖
            result++;
        }
        return result;
    }
};
```