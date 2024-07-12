---
title: What is Insertion Sort?
categories: [CS, Sorting]
tag: [CS, Sorting, Algorithm, Insertion Sort]
---

# Insertion Sort
- Builds the sorted array one item at a time by inserting elements in their correct position.
- **Time Complexity**: `O(N^2)`

```python
def insertion_sort(arr):
    for end in range(1, len(arr)):
        i = end
        while i > 0 and arr[i - 1] > arr[i]:
            arr[i - 1], arr[i] = arr[i], arr[i - 1]
            i -= 1
    return arr
```