---
title: 力扣 102. 二叉树的层序遍历
date: 2022/8/5 下午8:53
tags: [leetcode, bfs]
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

# bfs

```python
def levelOrder(root):
    if not root:
        return []
    res, cur = [], [root]
    while cur:
        tmp = []
        for _ in range(len(cur)):
            node = cur.pop(0)
            tmp.append(node.val)
            if node.left: cur.append(node.left)
            if node.right: cur.append(node.right)
        res.append(tmp)
    return res
```