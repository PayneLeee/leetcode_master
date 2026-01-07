# 2024/4/17 代码随想录Day 1：数组 704. 二分查找，27. 移除元素
数组理论基础，704. 二分查找，27. 移除元素  
## 数组理论基础

[随想录文章链接](https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html) 

## 703.二分查找
熟悉根据 左闭右开，左闭右闭 两种区间规则 写出来的二分法


[题目链接 703](https://leetcode.cn/problems/binary-search/) 给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。
### 第一次提交
不想思考边界条件，干脆直接都return 了一遍
#### c ++ Version
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            if (target == nums[left])
                return left;
            if (target == nums[right])
                return right;
            if (target < nums[left] || target > nums[right])
                return -1;
            int mid = (left + right) / 2;
            if (target == nums[mid])
                return mid;
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
```

#### Python Version
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        while left <= right:
            if target < nums[left] or target > nums[right]: return -1
            mid = int((left + right)/2)
            if target == nums[left]: return left
            if target == nums[right]: return right
            if target == nums[mid]: return mid
            if target > nums[mid]:
                left = mid + 1
            else:
                right = mid -1
        return -1
```
语法积累： len（listname）

### 复现学习优秀题解
[随想录](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html)
[视频链接](https://www.bilibili.com/video/BV1fA4y1o715)
：不用那么多提前判断，只需要分为左闭右闭，左闭右开两种
#### c++ version 
``` cpp
# 左闭右闭
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else if (target == nums[mid]) {
                return mid;
            }
        }
        return -1;
    }
};
# 左闭右开
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size();//# 这里有改动
        while (left < right) {//# 这里有改动
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid;
            } else if (target == nums[mid]) {
                return mid; //#这里有改动
            }
        }
        return -1;
    }
};
```
#### python Version
```python
# 左闭右闭
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = int(left)
            if target > nums[mid]:
                left = mid + 1
            elif target < nums[mid]:
                right = mid -1
            else:
                return mid
        return -1
# 左闭右开    
class Solution:
def search(self, nums: List[int], target: int) -> int:
    left, right = 0, len(nums)# 这里有改动
    while left < right:
        mid = int(left)
        if target > nums[mid]:
            left = mid + 1
        elif target < nums[mid]:
            right = mid
        else:
            return mid
    return -1
```
## 27. 移除元素
暴力的解法，可以锻炼一下我们的代码实现能力，建议先把暴力写法写一遍。

[题目链接 27.](https://leetcode.cn/problems/remove-element/) 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
0 <= nums.length <= 100
0 <= nums[i] <= 50
0 <= val <= 100
### 第一次提交
#### c ++ Version
办法比较取巧，利用了题目中给出了数字范围大于等于0，使用排序完成
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int count = 0;
        for (int i = 0 ; i < nums.size(); i++){
            if(nums[i] == val) {
                count ++;
                nums[i] = -1;
            }
        }
        int n = nums.size();
        sort(nums.begin(), nums.end(), greater<int>()); 
        #这里如何使用std中的sort需要记住
        return n - count;      
    }
};
```
#### Python Version
语法积累： list_name.sort(reverse=True)
```python 
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        count = 0
        for i in range(len(nums)):
            if nums[i] == val:
                count = count + 1
                nums[i] = -1
        nums.sort(reverse = True)
        return len(nums) - count
```
### 复现学习优秀题解
[随想录](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html) [视频链接](https://www.bilibili.com/video/BV12A4y1Z7LP): 快慢指针方法

#### c++ version
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast ++) {
            if (val != nums[fast]) {
                nums[slow++] = nums[fast];
            }
        }
        return slow;
    }
};
```
#### python version
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        for fast in range(len(nums)):
            if val != nums[fast]:
                nums[slow] = nums[fast]
                slow = slow + 1
            
        return slow
```            