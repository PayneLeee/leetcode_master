# 2024/5/23 Day37 greedy  860.柠檬水找零  406.根据身高重建队列  452. 用最少数量的箭引爆气球 

**遇到两个维度权衡的时候，一定要先确定一个维度，再确定另一个维度。如果两个维度一起考虑一定会顾此失彼。**

## 860.柠檬水找零
[题目链接 860](https://leetcode.cn/problems/lemonade-change/submissions/533983101/)

在柠檬水摊上，每一杯柠檬水的售价为 5 美元。顾客排队购买你的产品，（按账单 bills 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 5 美元、10 美元或 20 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 5 美元。

注意，一开始你手头没有任何零钱。

给你一个整数数组 bills ，其中 bills[i] 是第 i 位顾客付的账。如果你能给每位顾客正确找零，返回 true ，否则返回 false 。

### 提交
能用十元就用十元，没有记录20的意义，应为20不能用作找零

```cpp
class Solution {
public:
    int five = 0, ten = 0;
    bool lemonadeChange(vector<int>& bills) {
        for (int bill : bills) {
            if (bill == 5) {
                five++;
                continue;
            }
            if (bill == 10) {
                if (five > 0) {
                    five--;
                    ten++;
                    continue;
                }
                return false;
            }
            if (bill == 20) {
                if (five == 0) return false;
                if (ten > 0) {
                    five--;
                    ten--;
                    continue;
                }
                if (ten == 0 && five < 3) return false;
                five -= 3;
            }
        }
        return true;
    }
};
```

## 406.根据身高重建队列
[题目链接 406](https://leetcode.cn/problems/queue-reconstruction-by-height/description/)
假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

### 第一次提交
局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性

全局最优：最后都做完插入操作，整个队列满足题目队列属性
```cpp
class Solution {
public:
    class cmp {
        public:
        cmp(){}
        bool operator()(const vector<int> a, const vector<int> b) {
            if(a[0] > b[0]) return true;
            if(a[0] < b[0]) return false;
            if(a[0] == b[0]) return a[1] < b[1];
            return false;
        }
    };

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp());
        vector<vector<int>> res;
        for (vector<int> now : people) {
            res.insert(res.begin()+ now[1], now);
        }
        return res;
    }
};
```
### 学习题解
[随想录](https://programmercarl.com/0406.%E6%A0%B9%E6%8D%AE%E8%BA%AB%E9%AB%98%E9%87%8D%E5%BB%BA%E9%98%9F%E5%88%97.html#%E6%80%9D%E8%B7%AF)

如何使用链表加快insert操作
```cpp
int position = now[1];
std::list<vector<int>>::iterator it = res.begin();
while(position --) {
    it ++;
}
res.insert(it, now);
```

```cpp
class Solution {
public:
    class cmp {
        public:
        cmp(){}
        bool operator()(const vector<int> a, const vector<int> b) {
            if(a[0] > b[0]) return true;
            if(a[0] < b[0]) return false;
            if(a[0] == b[0]) return a[1] < b[1];
            return false;
        }
    };

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp());
        list<vector<int>> res;
        for (vector<int> now : people) {
            int position = now[1];
            std::list<vector<int>>::iterator it = res.begin();
            while(position --) {
                it ++;
            }
            res.insert(it, now);
        }
        return vector<vector<int>>(res.begin(), res.end());;
    }
};
```
##  452. 用最少数量的箭引爆气球 

[题目链接 452](https://programmercarl.com/0452.%E7%94%A8%E6%9C%80%E5%B0%91%E6%95%B0%E9%87%8F%E7%9A%84%E7%AE%AD%E5%BC%95%E7%88%86%E6%B0%94%E7%90%83.html) 
有一些球形气球贴在一堵用 XY 平面表示的墙面上。墙面上的气球记录在整数数组 points ，其中points[i] = [xstart, xend] 表示水平直径在 xstart 和 xend之间的气球。你不知道气球的确切 y 坐标。

一支弓箭可以沿着 x 轴从不同点 完全垂直 地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被 引爆 。可以射出的弓箭的数量 没有限制 。 弓箭一旦被射出之后，可以无限地前进。

给你一个数组 points ，返回引爆所有气球所必须射出的 最小 弓箭数 

### 第一次提交
排序之后顺序找，但是时间效率很低
```cpp
class Solution {
public:
    class cmp {
        public:
        cmp(){}
        bool operator()(vector<int> a,vector<int> b) {
            if(a[0] < b[0]) return true;
            else if (a[0] > b[0] ) return false;
            else return a[1] < b[1];
        }
    };
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(),cmp());
        int cnt = points.size();
        int arrow = points[0][1];
        for(vector<int> bo : points) {
            if (bo[0] > arrow) {
                arrow = bo[1];
            } else if (bo[1] < arrow) {
                arrow = bo[1];
                cnt --;
            } else {
                cnt --;
            }
        }
        return cnt + 1;
    }
};
```

### 学习题解
不需要那么精细的排序
[随想录](https://programmercarl.com/0452.%E7%94%A8%E6%9C%80%E5%B0%91%E6%95%B0%E9%87%8F%E7%9A%84%E7%AE%AD%E5%BC%95%E7%88%86%E6%B0%94%E7%90%83.html#%E6%80%9D%E8%B7%AF)

```cpp
class Solution {
private:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    }
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.size() == 0) return 0;
        sort(points.begin(), points.end(), cmp);

        int result = 1; // points 不为空至少需要一支箭
        int arrow = points[0][1];
        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] > arrow) {  // 气球i和气球i-1不挨着，注意这里不是>=
                arrow = points[i][1];
                result++; // 需要一支箭
            }
            else {  // 气球i和气球i-1挨着
                arrow = min(arrow, points[i][1]); // 更新重叠气球最小右边界
            }
        }
        return result;
    }
};
```