---
title: 力扣 141. 环形链表
date: 2022/8/9 上午10:49
tags: [leetcode, 链表]
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
def hasCycle(head):
    # 异步
    if not head or not head.next: return False
    s2, s1 = head.next, head
    while s1 != s2:
        if not s2 or not s2.next: return False
        s1 = s1.next
        s2 = s2.next.next
    return True
```