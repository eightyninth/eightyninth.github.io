---
title: 力扣 105. 从前序与中序遍历序列构造二叉树
date: 2022/8/6 上午11:18
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
def buildTree(preorder, inorder):
    if not preorder: return 
    root = TreeNode(preorder[0])
    root_idx = inorder.index(preorder.pop(0))
    root.left = self.buildTree(preorder[:root_idx], inorder[:root_idx])
    root.right = self.buildTree(preorder[root_idx:], inorder[root_idx + 1:])
    return root
```