---
title: What is Quick Sort?
categories: [CS, Sorting]
tag: [CS, Sorting, Algorithm, Quick Sort]
---

# Quick Sort
- Selects a pivot and partitions the array around the pivot, then recursively sorts the partitions.
- **Time Complexity**: `O(NlogN)` on average, `O(N^2)` in the worst case

# Implementation of Algorithm
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot, tail = arr[0], arr[1:]
    left_side = [x for x in tail if x <= pivot]
    right_side = [x for x in tail if x > pivot]

    return quick_sort(left_side) + [pivot] + quick_sort(right_side)
```