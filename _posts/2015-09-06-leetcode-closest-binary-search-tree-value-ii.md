---
layout: post
title: "Leetcode: Closest Binary Search Tree Value II"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#题目
给定一颗BST(root)和一个浮点数(target),一个整数(k), 找到这颗BST中值最接近target的k个值。

#分析
从中间开始往两边迭代，可以做到O(klogn)的时间和O(logn)(不算输入输出)的空间。
用coroutine来模拟iteration应该也算是个前无古人的做法了吧^ ^.
代码贴在下面，如果谁有问题的话，可以发邮件问我。

```python
#这部分代码是测试用的
class Tree:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

t7 = Tree(7)
t5 = t7.left = Tree(5)
t2 = t5.left = Tree(2)
t1 = t2.left = Tree(1)
t4 = t2.right = Tree(4)
t3 = t4.left = Tree(3)
t6 = t5.right = Tree(6)
t9 = t7.right = Tree(9)
t8 = t9.left = Tree(8)
t10 = t9.right = Tree(10)
```


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

#封装一下coroutine的generator
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
