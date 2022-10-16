---
title: 力扣 94. 二叉树的中序遍历
date: 2022/8/5 上午9:42
tags: [leetcode, 树]
categories: [leetcode]
---

# 中序遍历

```python
def inorderTraversal(root):
    def order(node):
        if not node:
            return
        order(node.left)
        res.append(node.val)
        order(node.right)
    res = []
    order(root)
    return res
```