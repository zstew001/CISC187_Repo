# Week 3/4 Sorting Pt. 2

# Question 1: Average-Case Analysis of Insertion Sort
### Demonstrate why insertion sort has an average-case time complexity of O(N²)
## For an array containing N elements, insertion sort divides the array into a sorted portion and unsorted portion. 
## We'll call the array myArray[N]. Initially, the sorted portion contains only myArray[0], while the unsorted portion contains myArray[1] through myArray[N-1]. Each iteration of insertion sort uses the first element in the unsorted portion of the array as the **key*. The **key* element is then compared with elements in the sorted portion and any element greater than the **key* are shifted one position to the right until the correct position is achieved. Insertion sort repeats this process until there are no more elements left in the unsorted array. The amount of comparisons and/or shifts this requires is approximately N/2 on average.

## Diagram and Summation equation displaying average case time-complexity:
[insertionSortAnalysis_20260920_0001.pdf](https://github.com/user-attachments/files/32445336/insertionSortAnalysis_20260920_0001.pdf)

# Question 2: Changing the Starting Position of Insertion Sort
# Part A: Start at i = 1.

## A = [5, 4, 3, 2, 1]
### key = 4
### A[0] >=< key?, Comparisons++
### 5 > 4 (True), Shifts++

## A = [4, 5, 3, 2, 1]: **Total - Comparisons: 1, Shifts: 1*
### key = 3
### A[1] >=< key?, Comparisons++
### 5 > 3 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 4 > 3 (True), Shifts++

## A = [3, 4, 5, 2, 1]: **Total - Comparisons: 3, Shifts: 3*
### key = 2
### A[2] >=< key?, Comparisons++
### 5 > 2 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 4 > 2 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 3 > 2 (True), Shifts++

## A = [2, 3, 4, 5, 1]: **Total - Comparisons: 6, Shifts: 6*
### key = 1
### A[3] >=< key?, Comparisons++
### 5 > 1 (True), Shifts++
### A[2] >=< key?, Comparisons++
### 4 > 1 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 3 > 1 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 2 > 1 (True), Shifts++

## A = [1, 2, 3, 4, 5]: **Total - Comparisons: 10, Shifts: 10*
## Total Counted Operations: 20

# Part B: Start at i = 2.

## A = [5, 4, 3, 2, 1]
### key = 3
### A[1] >=< key?, Comparisons++
### 4 > 3 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 5 > 3 (True), Shifts++

## A = [3, 4, 5, 2, 1]: **Total - Comparisons: 2, Shifts: 2*
### key = 2
### A[2] >=< key?, Comparisons++
### 5 > 2 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 4 > 2 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 3 > 2 (True), Shifts++

## A = [2, 3, 4, 5, 1]: **Total - Comparisons: 5, Shifts: 5*
### key = 1
### A[3] >=< key?, Comparisons++
### 5 > 1 (True), Shifts++
### A[2] >=< key?, Comparisons++
### 4 > 1 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 3 > 1 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 2 > 1 (True), Shifts++

## A = [1, 2, 3, 4, 5]: **Total - Comparisons: 9, Shifts: 9*
## Total Counted Operations: 18

# Part C: Start at i = 3.

## A = [5, 4, 3, 2, 1]
### key = 2
### A[2] >=< key?, Comparisons++
### 3 > 2 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 4 > 2 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 5 > 2 (True), Shifts++

## A = [2, 4, 3, 5, 1]: **Total - Comparisons: 3, Shifts: 3*
### key = 1
### A[3] >=< key?, Comparisons++
### 5 > 1 (True), Shifts++
### A[2] >=< key?, Comparisons++
### 3 > 1 (True), Shifts++
### A[1] >=< key?, Comparisons++
### 4 > 1 (True), Shifts++
### A[0] >=< key?, Comparisons++
### 2 > 1 (True), Shifts++

## A = [1, 2, 3, 4, 5]: **Total - Comparisons: 7, Shifts: 7*
## Total Counted Operations: 14

# Part D: Correctness.
## Explanation: Insertion sort normally begins at **i = 1* because only A[0] comes before it, so the sorted portion of the algorithm only contains 1 element to compare against. Starting at **i = 2* or **i = 3* skips elements that may not have been sorted yet. If the elements before 2 or 3 aren't sorted before running the algorithm, there is no guarantee the array will be fully sorted. In the array above, the only reason the shifts in starting position still resulted in a fully sorted array is because we started with the ***worst-case*** scenario. This won't always be the case and breaks the assumption that insertion sort makes: the elements before the key element are already sorted. While reducing the number of iterations improved the time-complexity in this specific scenario, it will result in partially unsorted outcomes in most other arrays. 

# Question 3: Improving a Search Algorithm
## Consider the following JavaScript function:
```JavaScript
function containsX(string) {
    foundX = false;

    for (let i = 0; i < string.length; i++) {
        if (string[i] === "X") {
            foundX = true;
        }
    }

    return foundX;
}
```
# Part A: Complexity Analysis
### 1. When "X" is the first character: O(N). The function finds "X" as the first string element then continues looping through i < string.length.
### 2. When "X" occurs somewhere in the middle: O(N). The function finds "X" in the middle of the algorithm but continues looping for the same reason as listed in (1).
### 3. When "X" is the final character: O(N). The function finds "X" as the last element of the string and has no more string elements to check.
### 4. The function continues looping after finding "X" because the return statement isn't contained within the for loop. As a result, the for loop continues iterating until i !< string.length. 
### 5. The best-case, average-case, and worst-case behavior of the original implementation is O(N) because no matter where "X" is in the string, the for loop examines all N elements.

# Part B: Improve the Algorithm
```JavaScript
function containsX(string) {
    foundX = false;

    for (let i = 0; i < string.length; i++) {
        if (string[i] === "X") {
            foundX = true;
            break; // Added break statement to allow the loop to end if "X" is found.
        }
    }

    return foundX;
}
```
# Part C: Analyze the Improved Version
### 1. Best-Case: O(1). When "X" is the first character, the function finds it immediately and the break statement ends the loop.
### 2. Average-Case: O(N). Most of the time, "X" will be somewhere in the middle of the string, so the number of comparisons may be less than N in the average case, but as N increases, the amount of checked elements will grow in proportion to N. 
### 3. Worst-Case: O(N). If "X" is the last character (or isn't in the string at all), the function will have to perform N operations. 
### Explanation: An early exit can improve the actual number of operations because the likelihood that the "X" in the string isn't the last character is high. Statistically, the average-case is roughly O(N/2), which definitely reduces the number of operations required but as the string grows larger, doesn't change the linear growth of the function's time-complexity. 

# Analysis and Reflection:
## Insertion sort has a quadratic time-complexity in the average and worst cases because every element may need to be compared and shifted multiple times. As shown in question 2, the key element may need to be compared and sorted against the sorted section of the array up to N times. Therefore, the operations required by Insertion sort grows in proportion to O(N²). Additionally, starting at element **i = 1* ensures that the array will always be fully sorted by the end of an Insertion sort algorithm. Starting beyond **i = 1* might skip elements that haven't been sorted. Another important distinction is the difference between reducing the number of operations an algorithm performs in certain cases doesn't mean it improves the algorithm's Big-O time-complexity. Big-O time-complexity simply identifies the pattern with which the algorithm grows as its dataset increases. With that said, two implementations may still have the same worst-case Big-O complexity but perform differently in practice. The improved JavaScript function in question 3 is a good example of this concept. The range of O(1) to O(N) could be extremely large depending on the value of N. One use of the function may result in a near-immediate response, while another use might require a million loops before exiting.  
