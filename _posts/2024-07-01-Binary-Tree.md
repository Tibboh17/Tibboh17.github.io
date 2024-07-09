---
title: What is Binary Tree?
categories: [CS, Data Structure]
tag: [CS, Data Structure]
---

# Binary Tree
- Each node can have a maximum of two children linked to it.
- The most used form in tree.

# Types of Binary Tree

#### Full Binary Tree
- Every node has 0 or 2 children.

#### Complete Binary Tree
- All the levels are completely filled except possibly the last level and the last level has all keys as left as possible.

#### Perfect Binary Tree
- All the internal nodes have two children and all leaf nodes are at the same level. 

#### Skewed Binary Tree
- Each node has either 0 or 1 child, and all nodes are positioned to the left or the right.
 
# Representations of Binary Tree

#### If the Numbering Starts from `0` to `n - 1` 
- **Root Node** = `0`
- **Parent** = `(p - 1) // 2` where **Child** = `p`
- **Left Child** = `2 * p + 1` where **Parent** = `p`
- **Right Child** = `2 * p + 2` where **Parent** = `p`

#### If the Numbering Starts from `1` to `n` 
- **Root Node** = `1`
- **Parent** = `p // 2` where **Child** = `p``
- **Left Child** = `2 * p` where **Parent** = `p`
- **Right Child** = `2 * p + 1` where **Parent** = `p`
