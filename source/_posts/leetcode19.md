---
title: 力扣 19. 删除链表的倒数第 N 个结点
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 预置

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
```

# 链表基本操作

```python
def removeNthFromEnd(head, n):
    if not head: return
    node_len, node_cur = 0, head
    while node_cur:
        node_cur = node_cur.next
        node_len += 1
    
    top = ListNode(next=head)

    left_n, cur = node_len - n, top
    # print(node_len, left_n)

    while left_n > 0:
        cur = cur.next
        left_n -= 1
    
    # print(cur.val)
    cur.next = cur.next.next

    return top.next
```


