---
title: 力扣 2. 两数相加
date: 2022-07-02 21:22:45
tags: [leetcode, 链表]
categories: [leetcode]
---

# 简单遍历链表

```python
a, b, cur_num, next_ = l1, l2, 0, 0
while a and b:
    total = a.val + b.val + next_
    cur_num = total % 10
    next_ = total // 10

    a.val = cur_num
    b.val = cur_num

    a = a.next
    b = b.next

if a:
    while a:
        total = a.val + next_
        cur_num = total % 10
        next_ = total // 10

        a.val = cur_num
        a = a.next
    res = l1
else:
    while b:
        total = b.val + next_
        cur_num = total % 10
        next_ = total // 10

        b.val = cur_num
        b = b.next
    res = l2

if next_ > 0:
    cur = res
    while cur.next:
        cur = cur.next
    cur.next = ListNode(next_)

return res
```
