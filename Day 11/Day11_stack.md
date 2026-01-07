# 2024/4/27 代码随想录Day 11：  20. 有效的括号 1047. 删除字符串中的所有相邻重复项 150. 逆波兰表达式求值

## 20.有效的括号
[题目链接 20](https://leetcode.cn/problems/valid-parentheses/description/)

比较经典比较简单, 小技巧在于见到左括号放入右括号可以减少判断。

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (char c : s) {
            if (c == '(') st.push(')');
            else if (c == '[') st.push(']');
            else if (c == '{') st.push('}');
            else {
                if (st.empty()) return false;
                else if (c == st.top()) st.pop();
                else return false;
            }
        }
        return st.empty();
    }
};
```
## 1047. 删除字符串中的所有相邻重复项

[题目链接 1047](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)

比较简单，没有什么惊喜，用string来模拟stack可以减少stack转化到string的过程。

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        string st = "";
        for(char c : s) {
            if(st.size() == 0) st.push_back(c);
            else if (st[st.size() - 1] ==  c) st.pop_back();
            else st.push_back(c);
        }
        return st;
    }
};
```
## 150. 逆波兰表达式求值
[题目链接 150](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)
根据 逆波兰表示法，求表达式的值。

有效的运算符包括 + ,  - ,  * ,  / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

说明：

整数除法只保留整数部分。 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

**比较经典，再写一遍，主要问题在把结果push入栈而不是单独存贮**

```cpp
class Solution {
public:
     int evalRPN(vector<string>& tokens) {
        stack<int> nums;
        for (string t: tokens) {
            if (t == "+" || t == "-" || t == "*" || t == "/") {
                int num1 = nums.top();
                nums.pop();
                int num2 = nums.top();
                nums.pop();
                if (t == "+") nums.push(num2 + num1);
                if (t == "-") nums.push(num2 - num1);
                if (t == "*") nums.push(num2 * num1);
                if (t == "/") nums.push(num2 / num1);

            } else {
                nums.push(stoi(t));
            }
        }
        return nums.top();
    }
};
```