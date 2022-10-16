---
title: 力扣 23. 合并K个升序链表
date: 2022/7/20 下午12:46
tags: [leetcode, 链表, 队列]
categories: [leetcode]
---

# 预置

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```


# 队列 + leetcode 21

```python
def mergeKLists(lists):
    def mergelal(l1, l2):
        if not l1: return l2
        if not l2: return l1
        top = ListNode()
        cur, c1, c2 = top, l1, l2
        while c1 and c2:
            if c1.val <= c2.val:
                cur.next, c1 = c1, c1.next
            else:
                cur.next, c2 = c2, c2.next
            cur = cur.next
        
        if c1: cur.next = c1
        if c2: cur.next = c2
        return top.next
    
    if not lists: return None

    while len(lists) > 1:
        lists.append(mergelal(lists.pop(0), lists.pop(0)))

    return lists[0] 
```

