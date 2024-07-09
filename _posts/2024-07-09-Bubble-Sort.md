---
title: What is Bubble Sort?
categories: [CS, Sorting]
tag: [CS, Sorting, Algorithm, Bubble Sort]
---

# Bubble Sort
- Compares adjacent elements and swaps them if they are in the wrong order.
- **Time Complexity**: `O(N^2)`

# Implementation of Algorithm
```python
def bubble_sort(arr):
    n = len(arr)

    for i in range(n - 1):
        for j in range(n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

    return arr
```