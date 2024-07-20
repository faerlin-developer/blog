---
layout: default
title: Notes
parent: Disjoint Set
grand_parent: Leetcode
has_children: false
permalink: /docs/leetcode/disjoint-set/notes
---



```python
class DisjointSet:

    def __init__(self, size):
        self.root = [i for i in range(size)]
        self.rank = [1 for i in range(size)]

    def find(self, x):
        if x == self.root[x]:
            return x

        self.root[x] = self.find(self.root[x])
        return self.root[x]

    def union(self, left, right):
        leftRoot = self.find(left)
        rightRoot = self.find(right)
        if leftRoot != rightRoot:
            if self.rank[leftRoot] < self.rank[rightRoot]:
                self.root[leftRoot] = rightRoot
            elif self.rank[leftRoot] > self.rank[rightRoot]:
                self.root[rightRoot] = leftRoot
            else:
                self.root[leftRoot] = rightRoot
                self.rank[rightRoot] += 1
```