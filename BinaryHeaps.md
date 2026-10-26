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
```
        90
       /  \
     70    80
    / \    / \
   20 40  60 50
```
### Index
```0 - 90
1 - 70
2 - 80
3 - 20
4 - 40
5 - 60
6 - 50
```
### Index 1: 
### Parent Index: 0
### Left Child Index: 3
### Right Child Index: 4

### Index 2:
### Parent Index: 0
### Left Child Index: 5
### Right Child Index: 6

### Index 4:
### Parent Index: 1
### Left Child Index: Empty
### Right Child Index: Empty

### Index 5:
### Parent Index: 2
### Left Child Index: Empty
### Right Child Index: Empty

## Part 4: Why Use an Array?
### The last node is stored at index: ```N-1```. This allows direct node access without needing to traverse the entire tree to locate a node. Newly inserted values are placed at index ```N``` (the next open index in the array). This helps to maintain the complete-tree condition because every new node is added left-to-right. After adding a new node, it can be moved upward to ensure the heap condition is maintained. Removing the final array element maintains the complete-tree structure because the last node will always be the right node on the final level. As a result, removing the final element won't create gaps between the remaining nodes. A linked-node implementation would make locating the last node more complicated because nodes aren't stored at a specific index. Without direct index access, locating the last node would require traversing the entire tree.

## Part 5-16: Implement a MaxHeap Class
```C++
#include <iostream>
#include <vector>

using namespace std;

class MaxHeap {
private:
    vector<int> heap;

    int parentIndex(int index) const;
    int leftChildIndex(int index) const;
    int rightChildIndex(int index) const;

    void trickleUp(int index);
    void trickleDown(int index);

public:
    bool empty() const;
    int size() const;

    int maximum() const;

    void insert(int value);
    bool removeMax();

    void display() const;
};

int MaxHeap::parentIndex(int index) const {
    return (index - 1) / 2;
}

int MaxHeap::leftChildIndex(int index) const {
    return 2 * index + 1;
}

int MaxHeap::rightChildIndex(int index) const {
    return 2 * index + 2;
}

void MaxHeap::trickleUp(int index) {
    while (index > 0) {
        int parent = parentIndex(index);

        if (heap[index] <= heap[parent]) {
            break;
        }

        swap(heap[index], heap[parent]);
        index = parent;
    }
}

void MaxHeap::trickleDown(int index) {
    while (leftChildIndex(index) < size()) {
        int left = leftChildIndex(index);
        int right = rightChildIndex(index);
        int largest = index;

        if (heap[left] > heap[largest]) {
            largest = left;
        }

        if (right < size() && heap[right] > heap[largest]) {
            largest = right;
        }

        if (largest == index) {
            break;
        }

        swap(heap[index], heap[largest]);
        index = largest;
    }
}

bool MaxHeap::empty() const {
    return heap.empty();
}

int MaxHeap::size() const {
    return heap.size();
}

int MaxHeap::maximum() const {
    if (empty()) {
        return -1;
    }

    return heap[0];
}

void MaxHeap::insert(int value) {
    heap.push_back(value);
    trickleUp(size() - 1);
}

bool MaxHeap::removeMax() {
    if (empty()) {
        return false;
    }

    heap[0] = heap.back();
    heap.pop_back();

    if (!empty()) {
        trickleDown(0);
    }

    return true;
}

void MaxHeap::display() const {
    for (int i = 0; i < size(); i++) {
        cout << heap[i] << " ";
    }

    cout << endl;
}

int main() {
    MaxHeap myHeap;

    cout << "Heap empty: " << (myHeap.empty() ? "Yes" : "No") << endl;
    cout << "Remove from empty heap: " << (myHeap.removeMax() ? "Success" : "Failed") << endl;

    myHeap.insert(45);
    cout << "After first insert: ";
    myHeap.display();

    myHeap.insert(25);
    myHeap.insert(85);
    myHeap.insert(15);
    myHeap.insert(65);
    myHeap.insert(35);
    myHeap.insert(20);

    cout << "After adding: ";
    myHeap.display();

    myHeap.insert(95);
    cout << "After inserting 95: ";
    myHeap.display();

    cout << "Max value: " << myHeap.maximum() << endl;
    cout << "Heap size: " << myHeap.size() << endl;

    myHeap.removeMax();
    cout << "After removing max: ";
    myHeap.display();

    while (myHeap.size() > 1) {
        myHeap.removeMax();
    }

    cout << "One value remaining: ";
    myHeap.display();

    cout << "Remove last value: " << (myHeap.removeMax() ? "Success" : "Failed") << endl;
    cout << "Heap empty: " << (myHeap.empty() ? "Yes" : "No") << endl;
    cout << "Remove from empty heap again: " << (myHeap.removeMax() ? "Success" : "Failed") << endl;

    cout << "Final heap: ";
    myHeap.display();

    return 0;
}
```
## Part 6 Analysis:
### 1. The root doesn't have a valid parent because it is the first node in the heap (index 0). There isn't a node above the root, so requiring a valid parent would only apply to an index > 0.
### 2. To determine whether a calculated child index exists, you can compare the calculated index to the size of the heap. If ```i < heap.size()```, then the child index exists. If not, the index would result in an array-bounds error.
### 3. These formulas work because the tree is complete and stored level by level, because it prevents gaps in the array. Every node's parent and child position can be directly calculated using their index.

## Part 7 Analysis:
### Final Array:
```
[90, 85, 80, 70, 40, 60, 50, 20]
```
### Final Tree:
```
        90
       /  \
     85    80
    / \    / \
   70 40  60 50
  /
 20
```

### Manual Trace Reasoning: 
### A new value needs to be inserted at the end of the array because the heap must maintain completeness. If a new value were added in another position, it could leave an empty space or change the order of the nodes. If a value is inserted at the end, it can move up without effecting the completeness of the tree.

## Part 8 Analysis:
### Reading the maximum is O(1) because the maximum is stored at index 0 and therefore can be accessed directly. Removing the maximum is O(log N) because the next maximum value might need to shift through multiple levels. The ```maximum()``` function I implemented returns ```-1``` for an empty heap to prevent invalid accesses. 

## Part 10 Analysis:
### Final Array:
```
[88, 70, 75, 60, 30, 40, 50, 20]
```

### Final Tree:
```
        88
       /  \
     70    75
    / \    / \
   60 30  40 50
  /
 20
```

## Part 11 Analysis:
### 1. ```90``` should be selected.
### 2. If ```20``` were swapped with ```70```, the heap condition wouldn't hold true because ```90``` > ```70``` which would be it's parent.
### 3. Choosing the larger child would preserve the max-heap condition because it guarantees that the parent is >= all children. 

## Part 12 Analysis:
### 1. If the trickle node has only a left child, the algorithm should compare current value with the left child and swap only if the value is greater than than the left child.
### 2. If the trickle node has no children, the algorithm ends because there are no additional comparisons or swaps necessary.

## Part 14 - Complexity Analysis:
### 1. Read Maximum
### Big(O): O(1)
### Explanation: The maximum is always at index 0 and therefore can be directly accessed.

### 2. Insert
### Big(O): O(log N)
### Explanation: Trickle-up ensures there is only one possible path to the root.

### 3. Remove Maximum
### Big(O): O(log N)
### Explanation: Trickle-down ensure there is only one possible path to the bottom of the tree.

### 4. Search for Arbitrary Value
### Big(O): O(N)
### Explanation: In a worst-case scenario, every node may need to be checked.

### 5. Access Last Node in Array
### Big(O): O(1)
### Explanation: The last node can be directly accessed at index: ```N-1```.

## Part 15 Analysis:
### A binary heap may still be the better overall structure for a priority queue because insertion is O(log N) compared to O(N) for an ordered array. Even though removing elements from an ordered array is faster, a heap could perform better if insertions and deletions are happening.

## Part 16 Analysis:
### 1. Insert the priorities into an initially empty max-heap in the order shown.
```
Task A (4):  [4]
Task B (9):  [9, 4]
Task C (2):  [9, 4, 2]
Task D (7):  [9, 7, 2, 4]
Task E (10): [10, 9, 2, 4, 7]
```
### 2. Final Heap:
```
        10
       /  \
      9    2
     / \
    4   7
```
### 3. Task E would be processed first. The highest priority is ```10```.
### 4. After removing ```10```, ```7``` becomes the root and would need to swap with ```9```, restoring the heap.
### 5. Task B would be processed next because it has the next highest priority ```9```.
### 6. A heap is suitable for this type of application because it ensures that the task with the highest priority can be accessed immediately without sorting through the entire tree.

## Analysis and Reflection:
### A heap must be complete because each level needs to be filled from left to right without any gaps. Unlike a Binary Search Tree, a max-heap only requires that each parent is >= its children. This makes searching for an arbitrary value inefficient because there isn't a way to determine which subtree contains the value. However, the maximum value is always stored at index 0, allowing it to be accessed immediately without searching through the heap. Insertion starts at the end of the array to maintain completeness, while deletion replaces the root with the last node. When inserting or removing a value, it may need to move up or down through the tree to restore the heap condition. Trickle-down selects the larger child because swapping with the smaller child can and may still violate the heap condition. Insertion and removal are both O(log N) because the values only need to move through one path of the tree rather than checking every node. The heap's weak ordering isn't really a disadvantage for priority queues because there isn't a need to keep every value sorted. Since the highest-priority value can be accessed immediately, the heap can still handle insertion and removal efficiently.










