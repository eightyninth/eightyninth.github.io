---
title: 力扣 85. 最大矩形
date: 2022/8/4 下午7:20
tags: [leetcode, 动态规划, 单调栈]
categories: [leetcode]
---

# 动态规划

```python
def maximalRectangle(matrix):
    if len(matrix) < 1 or len(matrix[0]) < 1:
        return 0
    
    m, n = len(matrix), len(matrix[0])

    dp = [[0] * n for _ in range(m)]

    for i in range(m):
        for j in range(n):
            if j == 0 and matrix[i][j] == "1":
                dp[i][j] = 1
            elif matrix[i][j] == "0":
                dp[i][j] = 0
            elif matrix[i][j] == "1":
                dp[i][j] = dp[i][j - 1] + 1
    # print(dp)
    # 可转化为84. 柱状图中最大的矩形
    ret = 0
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == "0":
                continue
            width, area = dp[i][j], dp[i][j]
            for k in range(i - 1, -1, -1):
                width = min(width, dp[k][j])
                area = max(area, (i - k + 1) * width)
            ret = max(ret, area)
    return ret
```

# 单调栈

```python
def maximalRectangle(matrix):
    if len(matrix) < 1 or len(matrix[0]) < 1:
        return 0
    
    m, n = len(matrix), len(matrix[0])

    dp = [[0] * n for _ in range(m)]

    for i in range(m):
        for j in range(n):
            if j == 0 and matrix[i][j] == "1":
                dp[i][j] = 1
            elif matrix[i][j] == "0":
                dp[i][j] = 0
            elif matrix[i][j] == "1":
                dp[i][j] = dp[i][j - 1] + 1
    # print(dp)
    # 可转化为84. 柱状图中最大的矩形
    ret = 0
    for j in range(n):
        left, right = [0] * m, [0] * m

        tmp = []
        for i in range(m):
            while tmp and dp[tmp[-1]][j] >= dp[i][j]:
                tmp.pop()
            left[i] = tmp[-1] if tmp else -1
            tmp.append(i)
        tmp = []
        for i in range(m - 1, -1, -1):
            while tmp and dp[tmp[-1]][j] >= dp[i][j]:
                tmp.pop()
            right[i] = tmp[-1] if tmp else m
            tmp.append(i)
        
        for i in range(m):
            ret = max(ret, (right[i] - left[i] - 1) * dp[i][j])
        
    return ret
```