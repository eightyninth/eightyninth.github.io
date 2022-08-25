---
title: 笔试题 基础计算器
date: 2022/7/24 下午10:25
tags: [笔试, 栈, hash表, 校招]
categories: [leetcode, 校招]
---

# 题目描述

实现一个表达式计算, 输入包括: 非负整数, "(", ")", "+", "-", "*", "/", 空格


示例
> "2 * ( 6 - 1 ) / 4"


输出
> 2


# 思路


实际上是中缀表达式的计算, 中缀转后缀计算


# 栈 + hash

```python
def cal(s):
    if not s: return 0
    prior = {"+": 0, "-": 0, "*": 1, "/": 1, "(": -1}
    stack, op = [], []
    num = None
    # 中缀 -> 后缀
    for i in range(len(s)):
        if s[i] != " ":
            if s[i].isdigit():
                num = 10 * num + int(s[i]) if num != None else int(s[i])
            else:
                if num != None:
                    stack.append(num)
                    num = None
                if op:
                    if s[i] != "(" and s[i] != ")":
                        while op and prior[s[i]] <= prior[op[-1]]:
                            stack.append(op.pop())
                        op.append(s[i])
                    elif s[i] == "(":
                        op.append(s[i])
                    else:
                        while op and op[-1] != "(":
                            stack.append(op.pop())
                        op.pop()
                else:
                    op.append(s[i])
    if num != None: stack.append(num)
    while op: stack.append(op.pop())

    # print(stack)
    # 计算后缀
    res = []
    for cur in stack:
        if type(cur) == int:
            res.append(cur)
        else:
            num2, num1 = res.pop(), res.pop()
            if cur == "+":
                tmp = num1 + num2
            elif cur =="-":
                tmp = num1 - num2
            elif cur == "*":
                tmp = num1 * num2
            else:
                tmp = num1 / num2
            res.append(tmp)

    return res[-1]
    
```


# 扩展: 前缀表达式的计算

前缀不需要转换, 利用**栈**对表达式从右到左扫描计算
