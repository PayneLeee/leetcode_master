# 2024/4/23 代码随想录Day 7：454.四数相加II 383. 赎金信  15. 三数之和 18. 四数之和 

##  454.四数相加II

四数相加

[题目链接 454](https://leetcode.cn/problems/4sum-ii/description/) 给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

0 <= i, j, k, l < n

nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
 
 
### 第一次提交
[随想录](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html)
[视频链接](https://www.bilibili.com/video/BV1Md4y1Q7Yh/)

不需要去重所以相对简单
#### c++ version 
``` cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums1.size(); i++){
            for (int j = 0; j < nums2.size(); j++){
                if (map.count(nums1[i] + nums2[j])) {
                    map[nums1[i] + nums2[j]]++;
                } else {
                    map[nums1[i] + nums2[j]] = 1;
                }
            }
        }
        int res = 0;
        for (int i = 0; i < nums3.size(); i++){
            for (int j = 0; j < nums4.size(); j++){
                if (map.count(-nums3[i] - nums4[j])) {
                    res += map[-nums3[i] - nums4[j]];
                } 
            }
        }
        return res;
    }
};
```
#### Python Version
```python
class Solution(object):
    def fourSumCount(self, nums1, nums2, nums3, nums4):
        # 使用字典存储nums1和nums2中的元素及其和
        hashmap = dict()
        for n1 in nums1:
            for n2 in nums2:
                if n1 + n2 in hashmap:
                    hashmap[n1+n2] += 1
                else:
                    hashmap[n1+n2] = 1
        
        # 如果 -(n1+n2) 存在于nums3和nums4, 存入结果
        count = 0
        for n3 in nums3:
            for n4 in nums4:
                key = - n3 - n4
                if key in hashmap:
                    count += hashmap[key]
        return count
```
## 383. 赎金信

[题目链接 383](https://leetcode.cn/problems/ransom-note/description/) 给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。

### 第一次提交
比较简单
```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int hash[26] = {0};
        for (char c : magazine) {
            hash[c - 'a']++;
        }
        for (char c: ransomNote){
            hash[c - 'a']--;
            if (hash[c - 'a'] < 0) return false;
        }
        return true;
    }
};
```
## 15. 三数之和
[随想录](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)
[视频](https://www.bilibili.com/video/BV1GW4y127qo/)

[题目链接](https://leetcode.cn/problems/3sum/description/) 给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请

你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

### 第一次提交
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tri;
        sort(nums.begin(), nums.end());

        for (int i = 0; i < nums.size() && nums[i] <= 0; i++) { // 剪枝
            if (i > 0 && nums[i] == nums[i - 1])
                continue; // 去重
            int left = i + 1, right = nums.size() - 1;
            while (left < right && nums[i] + nums[left] <= 0) { // 剪枝
                if (nums[left] + nums[right] + nums[i] == 0) {
                    tri.push_back(nums[i]);
                    tri.push_back(nums[left]);
                    tri.push_back(nums[right]);
                    res.push_back(tri);
                    tri.clear();
                    left++;
                    right--;
                    while (nums[left] == nums[left - 1] && left < right)
                        left++; // 去重
                    while (nums[right] == nums[right + 1] && left < right)
                        right--; // 去重
                    continue;
                }
                if (nums[left] + nums[right] + nums[i] < 0) {
                    left++;
                    continue;
                }
                if (nums[left] + nums[right] + nums[i] > 0) {
                    right--;
                    continue;
                }
            }
        }
        return res;
    }
};
```
这回第一次写就很优雅
## 18. 四数之和
[题目链接 18](https://leetcode.cn/problems/4sum/description/) 给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

0 <= a, b, c, d < n
a、b、c 和 d 互不相同
nums[a] + nums[b] + nums[c] + nums[d] == target
你可以按 任意顺序 返回答案 。

Trick： 如何处理超过int范围：（long）a+b
```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); k++) {
            if (nums[k] > target && nums[k] >= 0)
                break;//剪枝
            if (k > 0 && nums[k] == nums[k - 1])
                continue;//去重
            for (int i = k + 1; i < nums.size(); i++) {
                if (nums[k] + nums[i] > target && nums[k] + nums[i] >= 0)
                    break;//剪枝
                if (i > k + 1 && nums[i] == nums[i - 1])
                    continue;//去重
                int left = i + 1, right = nums.size() - 1;
                while (left < right &&
                       !((long)nums[k] + nums[i] + nums[left] > target &&
                         target >= 0 && nums[left] >= 0)) {//剪枝
                    if ((long)nums[k] + nums[i] + nums[left] + nums[right] ==
                        target) {
                        res.push_back(vector<int>{nums[k], nums[i], nums[left],
                                                  nums[right]});
                        left++;
                        right--;
                        while (nums[left] == nums[left - 1] && left < right)//注意这里的停止条件，容易死循环
                            left++;//去重
                        while (nums[right] == nums[right + 1] && left < right)//注意这里的停止条件，容易死循环
                            right--;//去重
                        continue;
                    }
                    if ((long)nums[k] + nums[i] + nums[left] + nums[right] <
                        target) {
                        left++;
                        continue;
                    }
                    if ((long)nums[k] + nums[i] + nums[left] + nums[right] >
                        target) {
                        right--;
                        continue;
                    }
                }
            }
        }
        return res;
    }
};
```