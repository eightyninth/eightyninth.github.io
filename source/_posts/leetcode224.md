---
title: 力扣 224. 基本计算器
date: 2022/7/25 上午10:58
tags: [leetcode, 栈]
categories: [leetcode]
---

# 栈处理op

```python
def cal(s):
    op = [1]
    sign, res = 1, 0
    i, s_len = 0, len(s)
    while i < s_len:
        if s[i] == " ":
            i += 1
        elif s[i] == "+":
            sign = op[-1]
            i += 1
        elif s[i] == "-":
            sign = -op[-1]
            i += 1
        elif s[i] == "(":
            op.append(sign)
            i += 1
        elif s[i] == ")":
            op.pop()
            i += 1
        else:
            num = 0
            while i < s_len and s[i].isdigit():
                num = num * 10 + ord(s[i]) - ord("0")
                i += 1
            res += sign * num
    
    return res   
```
