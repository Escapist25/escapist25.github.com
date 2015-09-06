---
layout: post
title: "Leetcode: Closest Binary Search Tree Value II"
description: ""
category: 
tags: []
---
{% include JB/setup %}

```python
def getBigger(root, val):
    if root == None:
        return
    if root.val > val:
        tl = getBigger(root.left,val)
        for i in tl:
            yield i
    if root.val > val:
        yield root.val
    tr = getBigger(root.right,val)
    for i in tr:
        yield i

def getSmaller(root,val):
    if root == None:
        return 
    if root.val <= val:
        tr = getSmaller(root.right,val)
        for i in tr:
            yield i
    if root.val <= val:
        yield root.val
    tl = getSmaller(root.left,val)
    for i in tl:
        yield i

class Iter:
    def __init__(self,obj):
        self.iter = obj
        self.cache = None
    def peek(self):
        if self.cache == None:
            try:
                self.cache = next(self.iter)
            except StopIteration:
                return None
        return self.cache
    def next(self):
        self.cache = None
        
def closer(a,b,tar):
    print a,b
    return b == None or (a != None and abs(a-tar) < abs(b-tar))
        
class Solution(object):
    def closestKValues(self, root, target, k):
        """
        :type root: TreeNode
        :type target: float
        :type k: int
        :rtype: List[int]
        """
        re = []
        l = Iter(getSmaller(root,target))
        r = Iter(getBigger(root,target))
        n = 0
        while n < k:
            l_value = l.peek()
            r_value = r.peek()
            if closer(l_value,r_value,target):
                print 'l'
                re.append(l_value)
                l.next()
            else:
                print 'r'
                re.append(r_value)
                r.next()
            n += 1
        return re
```
