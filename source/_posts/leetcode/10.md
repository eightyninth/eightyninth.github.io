---
title: 力扣 10. 正则表达式匹配
date: 2022-07-02 21:22:45
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划-二维数组

```python
def isMatch(s, p):
    def match(i, j):
        # 指针未指向字符串s
        if i == 0:
            return False
        # 单个字符匹配任意字符
        if p[j - 1] == ".":
            return True
        # 单个字符一对一匹配
        return s[i - 1] == p[j - 1]
    m, n = len(s), len(p)
    dp = [[False] * (n + 1) for _ in range(m + 1)]
    dp[0][0] = True

    for i in range(m + 1):
        for j in range(1, n + 1):
            # 零到多字符匹配
            if p[j - 1] == "*":
                dp[i][j] |= dp[i][j - 2] # 匹配零个
                if match(i, j - 1):     # 要进行匹配
                    dp[i][j] |= dp[i - 1][j]
            else:
                # 一对一字符匹配
                if match(i, j):
                    dp[i][j] |= dp[i - 1][j - 1]

    return dp[m][n]
```
