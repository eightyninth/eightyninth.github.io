---
title: 力扣 148. 排序链表
date: 2022/8/22 下午12:25
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

# 归并排序

```python
def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    if not head or not head.next: return head
    
    slow, fast = head, head.next
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
    
    mid, slow.next = slow.next, None
    left, right = self.sortList(head), self.sortList(mid)

    res = h = ListNode(0)

    while left and right:
        if left.val < right.val:
            h.next = left
            left = left.next
        else:
            h.next = right
            right = right.next
        h = h.next

    if left: h.next = left
    elif right: h.next = right
    return res.next
```