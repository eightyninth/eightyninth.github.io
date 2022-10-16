---
title: 力扣 104. 二叉树的最大深度
date: 2022/8/6 上午9:49
tags: [leetcode, dfs]
categories: [leetcode]
---

# 前置

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

# dfs

```python
def maxDepth(root):
    # dfs
    def dfs(node):
        if not node:
            return 0
        return max(dfs(node.left), dfs(node.right)) + 1
    return dfs(root) 
```