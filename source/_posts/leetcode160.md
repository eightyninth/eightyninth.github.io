---
title: 力扣 160. 相交链表
date: 2022/8/23 下午10:28
tags: [leetcode, 链表]
categories: [leetcode]
---

# 前置

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

# 解法

```python
def getIntersectionNode(headA, headB):
    a, countA, b, countB = headA, 0, headB, 0
    while a:
        countA += 1
        a = a.next
    while b:
        countB += 1
        b = b.next
    # print(countA, countB)
    if countA > countB:
        c, cur, other = countA - countB, headA, headB
    else:
        c, cur, other = countB - countA, headB, headA
    while c > 0:
        cur = cur.next
        c -= 1
    
    while cur != other:
        cur = cur.next
        other = other.next

    return cur
```