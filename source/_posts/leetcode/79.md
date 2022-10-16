---
title: 力扣 79. 单词搜索
date: 2022/8/3 上午11:44
tags: [leetcode, dfs, 回溯]
categories: [leetcode]
---

# 回溯思想 + dfs 实现

```python
def exist(board, word):
    def dfs(i, j, k):
        # 首先判断完成匹配与否
        if k == s_len:
            return True
        # 判断是否在数组内
        if i >= m or j >= n or i < 0 or j < 0:
            return False
        # 判断是否匹配过
        if dp[i][j] == True:
            return False
        # 判断是否匹配得上
        if board[i][j] != word[k]:
            return False 
        # 标记当前路线已匹配
        dp[i][j] = True
        # 匹配上了
        if dfs(i - 1, j, k + 1) or dfs(i + 1, j, k + 1) or dfs(i, j - 1, k + 1) or dfs(i, j + 1, k + 1):
            return True
        # 准备下一路线匹配
        dp[i][j] = False
        # 没匹配上
        return False
        
    m, n = len(board), len(board[0])
    dp = [[False] * n for _ in range(m)]
    k, s_len = 0, len(word)

    # 寻找起始点
    for i in range(m):
        for j in range(n):
            if board[i][j] == word[0]:
                # 找到起始点后, dfs
                if dfs(i, j, 0):
                    return True
    return False
```