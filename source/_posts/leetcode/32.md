---
title: 力扣 32. 最长有效括号
date: 2022/7/26 下午2:19
tags: [leetcode, 动态规划, 栈, 贪心]
categories: [leetcode]
---

# 动态规划

## 思想

dp[i] 表示到第i个字符的有效括号数量
初始情况下, 全部初始化为0


1. s[i - 1], s[i] == "()"
   dp[i] = dp[i - 2] + 2
2. s[i - 1], s[i] == "...))"
    dp[i] = dp[i - dp[i - 1] - 1] + dp[i - 2] + 2 if s[i - dp[i - 1] - 1] == "("
代码还需考虑 i 的范围的事情


## 代码


```python
def longestValidParentheses(s):
    # 动态规划
    s_len = len(s)
    dp, max_sub = [0 for _ in range(s_len)], 0
    for i in range(s_len):
        if s[i] == ")" and i - 1 >= 0:
            if s[i - 1] == "(":
                dp[i] = dp[i - 2] + 2 if i - 2 >= 0 else 2
            elif i - dp[i - 1] > 0 and s[i - dp[i - 1] - 1] == "(":
                dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2
        max_sub = max(max_sub, dp[i])
    # print(dp)
    return max_sub
```


# 栈


## 思想

1. 对于遇到的每个"(", 将它的下标放入栈中

2. 对于遇到的每个")", 先弹出栈顶元素表示匹配了当前右括号:

    1. 如果栈为空，说明当前的右括号为没有被匹配的右括号，将其下标放入栈中来更新我们之前提到的「最后一个没有被匹配的右括号的下标」
    2. 如果栈不为空，当前右括号的下标减去栈顶元素即为「以该右括号为结尾的最长有效括号的长度」


## 代码

```python
def longestValidParentheses(s):
    # 栈
    stack, max_sub = [-1], 0
    for i in range(len(s)):
        if s[i] == "(":
            stack.append(i)
        else:
            stack.pop()
            if stack:
                max_sub = max(max_sub, i - stack[-1])
            else:
                stack.append(i)
    
    return max_sub
```


# 贪心

```python
def longestValidParentheses(s):
    # 贪心
    left, right, max_sub, s_len = 0, 0, 0, len(s)
    for i in range(s_len):
        if s[i] == "(":
            left += 1
        else:
            right += 1
        
        if left == right:
            max_sub = max(max_sub, right * 2)
        elif right > left:
            left, right = 0, 0
    
    left, right = 0, 0
    for i in range(s_len - 1, -1, -1):
        if s[i] == "(":
            left += 1
        else:
            right += 1
        
        if left == right:
            max_sub = max(max_sub, right * 2)
        elif left > right:
            left, right = 0, 0
    
    return max_sub
```