# Binary Heaps

## Part 1: Is It a Valid Max-Heap?
### Heap A:
```
        90
       /  \
     70    80
    / \    /
   20 40  60
```
### 1. A is a valid max-heap.
### 2. Heap condition is satisfied: every parent > child.
### 3. Complete-tree condition is satisfied. All nodes are populated left-to-right without gaps.
### 4. No rules violated.

### Heap B:
```
        90
       /  \
     70    80
       \   /
       40 60
```
### 1. B is not a valid max-heap.
### 2. Heap condition is satisfied: every parent > child.
### 3. Complete-tree condition isn't satisfied. Node 70 has no left child.
### 4. Completeness is violated because node 70 has a right child without a left child. If 40 was the left child of 70 and 60 was the right child, the condition wouldn't be violated. 

### Heap C:
```
        90
       /  \
     95    80
    / \
   20 40
```
### 1. C is not a valid max-heap.
### 2. Heap condition isn't satisfied: 95 > 90. A child can't be > parent.
### 3. Complete-tree condition is satisfied. Nodes are filled left-to-right without gaps.
### 4. Heap condition is violated because parent must be >= child. If 90 and 95 were swapped, the condition wouldn't be violated.

## Part 2: Heap vs. Binary Search Tree
```
        100
       /   \
      80    70
     / \    / \
    20 60  50 40
```
### 1. This is a valid max-heap because every parent > child, and the tree is satisfies the complete-tree condition.
### 2. This isn't necessarily a Binary Search Tree because values to the right of ```100``` are < ```100```. A BST would require them to be > ```100```.
### 3. No, even though 40 < 100 but both subtrees could contain values smaller than 100. So there's know way to know which tree would contain 40 in a max-heap.
### 4. Arbitrary searching is generally O(N) in a heap because a heap doesn't guarantee which subtree contains smaller values. In a worst-case scenario, every node might need to be examined.
### 5. In a max-heap, the maximum value ```100``` can be accessed immediately.

## Part 3: Represent a Heap Using an Array

