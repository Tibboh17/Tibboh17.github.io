---
title: What is Merge Sort?
categories: [CS, Sorting]
tag: [CS, Sorting, Algorithm, Merge Sort]
---

# Merge Sort
- Divides the array into halves, recursively sorts them, and then merges the sorted halves.
- **Time Complexity**: `O(NlogN)`

# Implementation of Algorithm
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    sorted_arr = []
    i, j = 0, 0

    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            sorted_arr.append(left[i])
            i += 1
        else:
            sorted_arr.append(right[j])
            j += 1
    
    while i < len(left[i]):
        sorted_arr.append(left[i])
        i += 1

    while j < len(right[j]):
        sorted_arr.append(right[j])
        j += 1

    return sorted_arr     
```