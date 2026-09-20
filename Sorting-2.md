# Week 3/4 Sorting Pt. 2

# Question 1: Average-Case Analysis of Insertion Sort
### Demonstrate why insertion sort has an average-case time complexity of O(N²)
## For an array containing N elements, insertion sort divides the array into a sorted portion and unsorted portion. 
## We'll call the array myArray[N]. Initially, the sorted portion contains only myArray[0], while the unsorted portion contains myArray[1] through myArray[N-1]. Each iteration of insertion sort uses the first element in the unsorted portion of the array as the **key**. The **key** element is then compared with elements in the sorted portion and any element greater than the **key** are shifted one position to the right until the correct position is achieved. Insertion sort repeats this process until there are no more elements left in the unsorted array. The amount of comparisons and/or shifts this requires is approximately N/2 on average.

## Diagram and Summation equation displaying average case time-complexity:
[insertionSortAnalysis_20260920_0001.pdf](https://github.com/user-attachments/files/32445336/insertionSortAnalysis_20260920_0001.pdf)

# Question 2: Changing the Starting Position of Insertion Sort
### Part A: Start at i = 1.

## A = [5, 4, 3, 2, 1]
### key = 4
### A[0] >=< key?, Comparisons++
### 5 > 4 (True), Shifts++

## A = [4, 5, 3, 2, 1]: **Total - Comparisons: 1, Shifts: 1**
### key = 3
### A[1] >=< key?, Comparisons++
### 5 > 3 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 4 > 3 (True), Shifts++

## A = [3, 4, 5, 2, 1]: **Total - Comparisons: 3, Shifts: 3**
### key = 2
### A[2] >=< key?, Comparisons++
### 5 > 2 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 4 > 2 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 3 > 2 (True), Shifts++

## A = [2, 3, 4, 5, 1]: **Total - Comparisons: 6, Shifts: 6**
### key = 1
### A[3] >=< key?, Comparisons++
### 5 > 1 (True), Shifts++
### A[2] >=< key?, Comparisons++
### 4 > 1 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 3 > 1 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 2 > 1 (True), Shifts++

## A = [1, 2, 3, 4, 5]: **Total - Comparisons: 10, Shifts: 10**
## Total Counted Operations: 20
