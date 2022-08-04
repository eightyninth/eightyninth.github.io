---
title: 力扣 1143. 最长公共子序列
date: 2022/8/1 上午11:41
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划

```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1) + 1, len(text2) + 1

    dp = [[0] * n for _ in range(m)]

    for i in range(m):
        for j in range(n):
            if i != 0 and j != 0:
                if text1[i - 1] == text2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    # print(dp)
    return dp[-1][-1]
```