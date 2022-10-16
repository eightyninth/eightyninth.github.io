---
title: 力扣 146. LRU缓存
date: 2022/8/12 下午2:38
tags: [leetcode, hash表, 双向链表]
categories: [leetcode]
---

# 暴力

```python
"""
关键在于时间戳的处理
"""
class LRUCache:

    def __init__(self, capacity: int):
        self.cap = {}
        self.count = 0
        self.tim = []
        self.all = capacity

    def get(self, key: int) -> int:
        cur = self.cap.get(key, -1)
        if cur != -1:
            self.tim.pop(self.tim.index(key))
            self.tim.append(key)
        return cur

    def put(self, key: int, value: int) -> None:
        if key in self.cap.keys():
            self.tim.pop(self.tim.index(key))
            self.tim.append(key)
        else:
            if self.count < self.all:
                self.count += 1
            else:
                cur = self.tim.pop(0)
                del self.cap[cur]
            
            self.tim.append(key)
        self.cap[key] = value



# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```

# hash + 双向链表(只有虚拟头结点)
```python
"""
关键在于时间戳的处理, 官方建议用双向链表实现
"""
class Node:
    def __init__(self, key, val, nex=None, pre=None):
        self.key = key
        self.val = val
        self.next = nex
        self.pre = pre


class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.lis = Node(-1, -1)   
        self.values = {}

    def get(self, key: int) -> int:
        if key not in self.values:
            return -1
        node = self.values[key]
        self.remove(node)
        self.add_head(node)
        return node.val

    def put(self, key: int, value: int) -> None:
        if key not in self.values:
            node = Node(key, value)
            self.add_head(node)
            if self.size == self.capacity:
                del_key = self.remove_tail()
                del self.values[del_key]
            else:
                self.size += 1
            self.values[key] = node
        else:
            node = self.values[key]
            self.remove(node)
            node.val = value
            self.values[key] = node
            self.add_head(node)

    def remove(self, node):
        node.pre.next = node.next
        if node.next: node.next.pre = node.pre

    def add_head(self, node):
        cur = self.lis.next
        if cur:
            cur.pre.next = node
            node.next = cur
            node.pre = self.lis
            cur.pre = node
        else:
            self.lis.next = node
            node.pre = self.lis
    
    def remove_tail(self):
        cur = self.lis
        while cur.next:
            cur = cur.next
        self.remove(cur)
        return cur.key
```

# hash + 双向链表(存在虚拟头结点和虚拟尾节点)
```python
"""
关键在于时间戳的处理, 官方建议用双向链表实现
"""
class Node:
    def __init__(self, key, val, nex=None, pre=None):
        self.key = key
        self.val = val
        self.next = nex
        self.pre = pre


class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.start = Node(-1, -1)   
        self.end = Node(-1, -1)
        self.start.next = self.end
        self.end.pre = self.start
        self.values = {}

    def get(self, key: int) -> int:
        if key not in self.values:
            return -1
        node = self.values[key]
        self.remove(node)
        self.add_tail(node)
        return node.val

    def put(self, key: int, value: int) -> None:
        if key not in self.values:
            node = Node(key, value)
            self.add_tail(node)
            if self.size == self.capacity:
                del_key = self.remove_head()
                del self.values[del_key]
            else:
                self.size += 1
            self.values[key] = node
        else:
            node = self.values[key]
            self.remove(node)
            node.val = value
            self.values[key] = node
            self.add_tail(node)

    def remove(self, node):
        node.pre.next = node.next
        node.next.pre = node.pre

    def add_tail(self, node):
        self.end.pre.next = node
        node.next = self.end
        node.pre = self.end.pre
        self.end.pre = node
    
    def remove_head(self):
        cur = self.start.next
        self.remove(cur)
        return cur.key
```

