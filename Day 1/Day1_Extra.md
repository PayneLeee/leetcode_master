# 2024/4/17 代码随想录Day 1：数组 35.搜索插入 和 34. 查找元素的首尾位置
## 35.搜索插入
[题目链接](https://leetcode.cn/problems/search-insert-position/description/) 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
请必须使用时间复杂度为 O(log n) 的算法。

### 第一次提交
#### C++ Version
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while(left <= right){
            if (nums[left] > target) return left;
            if (nums[right] < target) return right + 1;            
            int mid = left + (right - left)/2;
            if(nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            }
        }
        return -1;
        // # 没有找到        
    }
};
```
### 复现学习优秀题解
没有找到时左边就是所求位置
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while(left <= right){       
            int mid = left + (right - left)/2;
            if(nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            }
        }
        return left;  
    }
};
```
## 34. 在排序数组中查找元素的第一个和最后一个位置
[题目链接](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/) 给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。
如果数组中不存在目标值 target，返回 [-1, -1]。
你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。

### 第一次提交
利用整数性质，结合35题寻找合适插入位置
#### C++ Version
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res;
        float left = target - 0.5;
        float right = target + 0.5;
        int first = find(nums, left);
        int last = find(nums, right);
        if (first < last) {
            res.push_back(first);
            res.push_back(last - 1);      
        } else {
            res.push_back(-1);
            res.push_back(-1);
        }
        return res;
    };

    int find(vector<int>& nums,  float target) {
        int left = 0, right = nums.size()-1;
        cout << left << " "<< right << endl;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};
```
### 复现学习优秀题解
[调整边界](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/solutions/504484/zai-pai-xu-shu-zu-zhong-cha-zhao-yuan-su-de-di-3-4/)
调整左右边界可以自然找到左右的边界点
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res{-1, -1};
        res[0] = findLeft(nums, target);
        //find second
        res[1] = findRight(nums, target);
        if (res[0] > res [1]) {
            res[0] = -1;
            res[1] = -1;
        }
        return res;
    };
    int findLeft(vector<int>& nums, int target){
        int left = 0, right = nums.size()-1;
        //find first
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return right + 1;
    }
    int findRight(vector<int>& nums, int target){
        int left = 0, right = nums.size()-1;
        //find first
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] <= target) {// right
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return right;
    }
       
};
```
[使用stl内置函数](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/solutions/2724102/c-stl-nei-zhi-han-shu-7xing-miao-sha-mo-14tk4/)
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if (!nums.size()) return {-1, -1};
        auto index1 = lower_bound(nums.begin(), nums.end(),target); // 返回第一个大于等于迭代器
        auto index2 = upper_bound(nums.begin(), nums.end(),target); // 返回第一个大于tar的迭代器
        if (index1 == nums.end() || *index1 != target) return {-1, -1}; // 这个值根本不存在
        return {(int)distance(nums.begin(), index1),(int)distance(nums.begin(), index2 - 1)};
    }
};

