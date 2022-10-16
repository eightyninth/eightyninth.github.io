---
title: 力扣 101. 对称二叉树
date: 2022/8/5 下午7:07
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
def isSymmetric(root):
    def order(left, right):
        if not left and not right: return True
        if not left or not right: return False
        return left.val == right.val and order(left.left, right.right) and order(left.right, right.left)
    
    return order(root, root)
```