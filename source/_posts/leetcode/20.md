---
title: 力扣 20. 有效的括号
date: 2022/7/20 上午10:11
tags: [leetcode, 栈, hash表]
categories: [leetcode]
---


# 栈 + hash表

```python
def isValid(s):
    if not s: return True 
    pip = {"}": "{",
           ")": "(",
           "]": "["}

    queue = []

    for sign in s:
        if sign in pip.keys():
            if not queue:
                return False
            else:
                cur = queue.pop()
                if pip[sign] != cur:
                    return False
        else:
            queue.append(sign)
        
    
    if not queue:
        return True
    else:
        return False
```