# Week 3/4 Sorting Pt. 2

# Question 1: Average-Case Analysis of Insertion Sort
### Demonstrate why insertion sort has an average-case time complexity of O(N²)
## For an array containing N elements, insertion sort divides the array into a sorted portion and unsorted portion. 
## We'll call the array myArray[N]. Initially, the sorted portion contains only myArray[0], while the unsorted portion contains myArray[1] through myArray[N-1]. Each iteration of insertion sort uses the first element in the unsorted portion of the array as the **key**. The **key** element is then compared with elements in the sorted portion and any element greater than the **key** are shifted one position to the right until the correct position is achieved. Insertion sort repeats this process until there are no more elements left in the unsorted array. The amount of comparisons and/or shifts this requires is approximately N/2 on average.

## Diagram and Summation equation displaying average case time-complexity:

[insertionSortAnalysis_20260920_0001.pdf](https://github.com/user-attachments/files/32445336/insertionSortAnalysis_20260920_0001.pdf)

