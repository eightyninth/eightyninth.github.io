---
title: 力扣 49. 字母异位词分组
date: 2022/7/30 下午9:56
tags: [leetcode, hash表]
categories: [leetcode]
---

# 排序 + hash表

```python
def groupAnagrams(strs):
    dic = {}
    for s in strs:
        cur = "".join(sorted(s))
        if cur in dic.keys():
            dic[cur].append(s)
        else:
            dic[cur] = [s]
    
    return list(dic.values())    
```