---
title: 力扣 114. 二叉树展开为链表
date: 2022/8/6 上午11:21
tags: [leetcode, 树]
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

# 解法

```python
def flatten(root):
    """
    Do not return anything, modify root in-place instead.
    """
    def order(node):
        if not node:
            return
        order(node.left)
        order(node.right)
        if node.left:
            next_ = node.right if node.right else None
            node.left, node.right = None, node.left
            if next_:
                cur = node.right
                while cur.right:
                    cur = cur.right
                cur.right = next_
    order(root)
```