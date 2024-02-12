---
layout: default
title: 724 - Find Pivot Index
parent: Arrays and String
grand_parent: Leetcode
has_children: false
permalink: /docs/leetcode/arrays-and-string/
---

# 724 - Find Pivot Index

Given an array of integers `nums`, calculate the pivot index of this array.

The __pivot index__ is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right. If the index is on the left edge of the array, then the left sum is 0 because there are no elements to the left. This also applies to the right edge of the array.

Return the leftmost pivot index. If no such index exists, return -1.

Example 1
```python
index = pivot_index([1, 7, 3, 6, 5, 6])
assert index == 3
```

Example 2
```python
# Example 2
index = pivot_index([1, 2, 3])
assert index == -1
```

Example 3
```python
# Example 3
index = pivot_index([2, 1, -1])
assert index == 0
```

_Click [here][Leetcode] to go to the corresponding problem entry in Leetcode.com._

## Solution

Let `N` be the length of the list of numbers `nums`.

Create two arrays called `left` and `right` with the same length as `nums`. 

Populate the entries of the `left` and `right` arrays such that

$$ 
\begin{split}
\text{left[i]} &= \text{nums[0]} + \text{nums[1]} + \dots + \text{nums[i-1]} \\
\text{right[i]} &= \text{nums[i+1]} + \text{nums[i+2]} + \dots + \text{nums[N-1]}
\end{split}
$$

If it exists, the index `i` where `left[i] == right[i]` is the pivot index.

```python
def pivot_index(nums):
    """Find the pivot index of the given list of numbers."""

    # Initialize left and right arrays with zero entries
    N = len(nums)
    left = [0 for _ in range(N)]
    right = [0 for _ in range(N)]

    # Compute entries of forward array
    # forward[0] is already equal to 0 so we start the loop at index 1
    for i in range(1, N):
        left[i] = left[i - 1] + nums[i - 1]

    # Compute entries of backward array
    # backward[N-1] is already already equal to zero so we start at index N-2
    for i in range(N - 2, -1, -1):
        right[i] = right[i + 1] + nums[i + 1]

    # Find the pivot
    # The pivot is index such that forward[i] is equal to backward[i]
    pivot = -1
    for i in range(N):
        if left[i] == right[i]:
            pivot = i
            break

    return pivot
```




----

[Leetcode]: https://leetcode.com/problems/find-pivot-index/description/?envType=study-plan&id=level-1

