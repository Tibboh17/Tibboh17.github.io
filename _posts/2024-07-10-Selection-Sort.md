---
title: What is Selection Sort?
categories: [CS, Sorting]
tag: [CS, Sorting, Algorithm, Selection Sort]
---

# Selection Sort
- Repeatedly finds the minimum element from the unsorted part and moves it to the sorted part.
- **Time Complexity**: `O(N^2)`

# Implemetation of Algorithm
```python
def selection_sort(arr):
    for i in range(len(arr) - 1):
        min_idx = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    
    return arr
```