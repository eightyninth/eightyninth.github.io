---
title: 力扣 21. 合并两个有序链表
date: 2022/7/20 上午10:23
tags: [leetcode, 链表]
categories: [leetcode]
---

# 预置

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

# 虚拟头节点 + 链表基础操作

```python
def mergeTwoLists(list1, list2):
    if not list1: return list2
    if not list2: return list1
    top = ListNode()
    cur1, cur2, cur = list1, list2, top
    while cur1 and cur2:
        if cur1.val <= cur2.val:
            cur.next, cur1 = cur1, cur1.next
        else:
            cur.next, cur2 = cur2, cur2.next
        cur = cur.next
    
    if cur1: cur.next = cur1
    if cur2: cur.next = cur2
    return top.next
```