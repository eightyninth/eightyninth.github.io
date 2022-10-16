---
title: 力扣 139. 单词拆分
date: 2022/8/9 上午9:50
tags: [leetcode, bfs, dfs, 动态规划]
categories: [leetcode]
---

# dfs

```python
def wordBreak(s, wordDict):
    # dfs
    def dfs(start):
        if start == s_len: return True

        if cur.get(start, -1) != -1: return cur.get(start, -1)

        for i in range(start + 1, s_len + 1):
            s_c = s[start: i]
            if s_c in word_set and dfs(i):
                cur[start] = 1
                return True
        cur[start] = 0
        return False 
    
    s_len = len(s)
    word_set, cur = set(wordDict), {}
    return dfs(0)
```

# bfs

```python
def wordBreak(s, wordDict):
    # bfs
    i, s_len = 1, len(s)
    word_set, cur, queue = set(wordDict), set(), [0]
    while queue:
        start = queue.pop(0)
        if start in cur: continue
        for i in range(start + 1, s_len + 1):
            if s[start: i] in word_set:
                if i == s_len: return True
                queue.append(i)
                
        cur.add(start)
    return False
```

# 动态规划

```python
def wordBreak(s, wordDict):
    # 动态规划
    s_len = len(s)
    dp, wordSet = [False] * (s_len + 1), set(wordDict)
    dp[0] = True
    for i in range(1, s_len + 1):
        for j in range(0, i):
            cur = s[j: i]
            if dp[j] and cur in wordSet:
                dp[i] = True
                break

    return dp[-1]
```

# 动态规划优化

```python
def wordBreak(s, wordDict):
    # 动态规划优化
    s_len = len(s)
    dp, wordSet = [False] * (s_len + 1), set(wordDict)
    dp[0] = True
    for i in range(1, s_len + 1):
        for j in range(0, i):
            if dp[i]:
                    break
            if not dp[j]:
                continue
            cur = s[j: i]
            if dp[j] and cur in wordSet:
                dp[i] = True
                break

    return dp[-1]
```