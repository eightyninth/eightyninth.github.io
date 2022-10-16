---
title: 力扣 72. 编辑距离
date: 2022/8/1 上午11:14
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划

![编辑距离: 动态规划](/images/leetcode/leetcode72.jpg)

```python
def minDistance(word1, word2):
    m, n = len(word1), len(word2)
     
    if m * n == 0:
        return m + n
    
    dp = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(n + 1):
        for j in range(m + 1):
            if i == 0 and j == 0:
                dp[i][j] = 0
            elif i == 0:
                dp[i][j] = dp[i][j - 1] + 1
            elif j == 0:
                dp[i][j] = dp[i - 1][j] + 1
            else:
                if word2[i - 1] != word1[j - 1]:
                    dp[i][j] = min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]) + 1
                else:
                    dp[i][j] = dp[i - 1][j - 1]
    # print(dp)
    return dp[-1][-1]
```