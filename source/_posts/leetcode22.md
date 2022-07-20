---
title: 力扣 22. 括号生成
date: 2022/7/20 上午10:44
tags: [leetcode, dfs, 回溯]
categories: [leetcode]
---

# 回溯思想 + 深度优先遍历(dfs)实现

```python
def generateParenthesis(n):
    res, cur = [], ""
    def dfs(cur, left, right):
        if left == 0 and right == 0:
            res.append(cur)
        elif right < left:
            return
        else:
            if left > 0:
                dfs(cur + "(", left - 1, right)
            if right > 0:
                dfs(cur + ")", left, right - 1)
    dfs(cur, n, n) 
    return res
```
