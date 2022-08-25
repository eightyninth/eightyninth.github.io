---
title: 力扣 155. 最小栈
date: 2022/8/23 下午8:40
tags: [leetcode, 栈]
categories: [leetcode]
---

# 栈

```python
import collections
class MinStack:

    def __init__(self):
        self.stack = collections.deque()
        self.min_stack = collections.deque()

    def push(self, val: int) -> None:
        self.stack.append(val)
        if not self.min_stack or self.min_stack[-1] >= val:
            self.min_stack.append(val)

    def pop(self) -> None:
        cur = self.stack.pop()
        if cur == self.min_stack[-1]: self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```