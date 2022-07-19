---
title: 力扣 17. 电话号码的字母组合
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 暴力解法

```python
def letterCombinations(digits):
    if not digits:
        return []
        
    hash_ = {"2": ["a", "b", "c"],
             "3": ["d", "e", "f"],
             "4": ["g", "h", "i"],
             "5": ["j", "k", "l"],
             "6": ["m", "n", "o"],
             "7": ["p", "q", "r", "s"],
             "8": ["t", "u", "v"],
             "9": ["w", "x", "y", "z"]}
    
    tmp, res = [""], []

    for d in digits:
        if d in hash_:
            for t in tmp:
                for ad in hash_[d]:
                    res.append(t + ad)
            # res += add_
            tmp = res
            res = []

    return tmp
```

2. 回溯 + 队列

```python
def letterCombinations(digits):
    if not digits:
        return []

    hash_ = {"2": ["a", "b", "c"],
             "3": ["d", "e", "f"],
             "4": ["g", "h", "i"],
             "5": ["j", "k", "l"],
             "6": ["m", "n", "o"],
             "7": ["p", "q", "r", "s"],
             "8": ["t", "u", "v"],
             "9": ["w", "x", "y", "z"]}
    
    tmp, res, digit_len = [], [], len(digits)

    def back(idx, digit_len):
        if idx == digit_len:
            res.append("".join(tmp))
        else:
            digit = digits[idx]
            for d in hash_[digit]:
                tmp.append(d)
                back(idx + 1, digit_len)
                tmp.pop()
        

    back(0, digit_len)
    return res
```


