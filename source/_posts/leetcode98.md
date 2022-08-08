---
title: 力扣 98. 验证二叉搜索树
date: 2022/8/5 上午11:36
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

# 前序遍历

```python
def isValidBST(root):
    # 前序遍历
    def order(node, lower, upper):
        if not node:
            return True

        val = node.val

        if val <= lower or val >= upper:
            return False
        
        if not order(node.left, lower, val):
            return False
        if not order(node.right, val, upper):
            return False
        
        return True

    ret = order(root, float("-inf"), float("inf"))
    return ret
```