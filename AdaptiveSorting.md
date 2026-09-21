# Week 3/4 Adaptive Sorting:

# Part A: Adaptive Sorting Selection - Step 1 - 3
```C++
#include <iostream>
using namespace std;

// Counts adjacent elements that aren't in order
int countAdjacentDisorder(int myArray[], int arraySize) {
    int disorderCount = 0;

    for (int currentIndex = 0; currentIndex < arraySize - 1; currentIndex++) {
        if (myArray[currentIndex] > myArray[currentIndex + 1]) {
            disorderCount++;
        }
    }

    return disorderCount;
}

// Function impl for selection sort
void selectionSort(int myArray[], int arraySize) {
    for (int currentIndex = 0; currentIndex < arraySize - 1; currentIndex++) {
        int minimumIndex = currentIndex;

        for (int comparisonIndex = currentIndex + 1; comparisonIndex < arraySize; comparisonIndex++) {
            if (myArray[comparisonIndex] < myArray[minimumIndex]) {
                minimumIndex = comparisonIndex;
            }
        }

        int temporaryValue = myArray[currentIndex];
        myArray[currentIndex] = myArray[minimumIndex];
        myArray[minimumIndex] = temporaryValue;
    }
}

// Function impl for insertion sort
void insertionSort(int myArray[], int arraySize) {
    for (int currentIndex = 1; currentIndex < arraySize; currentIndex++) {
        int currentValue = myArray[currentIndex];
        int previousIndex = currentIndex - 1;

        while (previousIndex >= 0 && myArray[previousIndex] > currentValue) {
            myArray[previousIndex + 1] = myArray[previousIndex];
            previousIndex--;
        }

        myArray[previousIndex + 1] = currentValue;
    }
}

// Function that cleanly prints the array
void displayArray(int myArray[], int arraySize) {
    for (int currentIndex = 0; currentIndex < arraySize; currentIndex++) {
        cout << myArray[currentIndex] << "\t";

        if ((currentIndex + 1) % 10 == 0) {
            cout << endl;
        }
    }
}

int main() {

    const int arraySize = 50;

    int myArray[arraySize] = {
        1, 2, 5, 4, 3,
        6, 8, 7, 10, 9,
        11, 14, 13, 12, 15,
        18, 17, 16, 20, 19,
        21, 23, 22, 25, 24,
        27, 26, 29, 28, 30,
        31, 34, 33, 32, 35,
        37, 36, 39, 38, 40,
        42, 41, 44, 43, 45,
        47, 46, 49, 48, 50
    }; // Test array (Partially Ordered)

    cout << "Array before sorting:" << endl;
    displayArray(myArray, arraySize);

    // Checks the initial array to determine how disordered it is
    int disorderCount = countAdjacentDisorder(myArray, arraySize);

    int totalAdjacentPairs = arraySize - 1;

    // Calculates the percentage of adjacent pairs that are out of order
    double disorderPercentage =
        (static_cast<double>(disorderCount) / totalAdjacentPairs) * 100;

    cout << "\nAdjacent disorder count: "
        << disorderCount << " out of " << totalAdjacentPairs << endl;

    cout << "Disorder percentage: "
        << disorderPercentage << "%" << endl;

    // Classifies the array and selects an algorithm based on % of disordered pairs
    if (disorderPercentage <= 10.0) {
        cout << "Array Type: Nearly Sorted" << endl;
        cout << "Selected algorithm: Insertion Sort" << endl;

        insertionSort(myArray, arraySize);
    }
    else if (disorderPercentage <= 40.0) {
        cout << "Array Type: Partially Ordered" << endl;
        cout << "Selected algorithm: Insertion Sort" << endl;

        insertionSort(myArray, arraySize);
    }
    else {
        cout << "Array Type: Highly Disordered" << endl;
        cout << "Selected algorithm: Selection Sort" << endl;

        selectionSort(myArray, arraySize);
    }

    cout << "\nArray after sorting:" << endl;
    displayArray(myArray, arraySize);

    return 0;
}
```
# Explanation: 
## 1. The program determines how ordered the array is before sorting. This is measured by checking adjacent elements against each other.
## The function: ```countAdjacentDisorder``` compares ```currentIndex``` and ```currentIndex + 1``` to determine how many pairs are out of order. When a pair is detected, ```disorderCount``` increments.
## I decided to base the disorder thresholds on the following percentages.
### - 0 - 10% is considered nearly sorted
### - 10% - 40% is considered partially ordered
### - Greater than 40% is highly disordered
## The lower two bounds rely on Insertion sort because it only requires a partial reordering of the data. In cases where the data is already partially sorted, especially in smaller datasets such as this one, Insertion sort can perform much closer to O(N) time-complexity. However, once 20 or more of the 49 pairs are out of order, (~40%), Insertion sort begins to lose any of it's potential upside. In the case of >40% disorder, Selection sort will perform more consistently.  

# Part B: Case Classification Without Sorting
```C++
#include <iostream>
using namespace std;

// Counts adjacent elements that aren't in order
int countAdjacentDisorder(int myArray[], int arraySize) {
    int disorderCount = 0;

    for (int currentIndex = 0; currentIndex < arraySize - 1; currentIndex++) {
        if (myArray[currentIndex] > myArray[currentIndex + 1]) {
            disorderCount++;
        }
    }

    return disorderCount;
}

// Function that cleanly prints the array
void displayArray(int myArray[], int arraySize) {
    for (int currentIndex = 0; currentIndex < arraySize; currentIndex++) {
        cout << myArray[currentIndex] << "\t";

        if ((currentIndex + 1) % 10 == 0) {
            cout << endl;
        }
    }
}

int main() {

    const int arraySize = 50;

    int myArray[arraySize];

    // User input for 50 integers
    cout << "Enter 50 integers:" << endl;

    for (int currentIndex = 0; currentIndex < arraySize; currentIndex++) {
        cin >> myArray[currentIndex];
    }

    cout << "\nOriginal array:" << endl;
    displayArray(myArray, arraySize);

    // Checks the original array to determine how disordered it is
    int disorderCount = countAdjacentDisorder(myArray, arraySize);

    int totalAdjacentPairs = arraySize - 1;

    // Calculates the percentage of adjacent pairs that are out of order
    double disorderPercentage =
        (static_cast<double>(disorderCount) / totalAdjacentPairs) * 100;

    cout << "\nAdjacent disorder count: "
        << disorderCount << " out of " << totalAdjacentPairs << endl;

    cout << "Disorder percentage: "
        << disorderPercentage << "%" << endl;

    // Classifies the array based on % of disordered pairs
    if (disorderPercentage <= 10.0) {
        cout << "Input classification: Best/Nearly Sorted" << endl;
    }
    else if (disorderPercentage <= 40.0) {
        cout << "Input classification: Average/Partially Ordered" << endl;
    }
    else {
        cout << "Input classification: Worst/Highly Reverse-Ordered" << endl;
    }

    return 0;
}
```
# Part C: Complexity of the Classification
## Assume the array contains N elements:
### 1. In an array containing N elements, there are N - 1 adjacent pairs. The loop to examine pairs begins at i = 0 and ends at N - 2, meaning every pair is examined only once. 
### 2. The operations change as N increases in proportion with N. For example, an array with 50 elements will make 49 comparisons, while an array with 100 elements will make 99 comparisons.
### 3. The Big-O time complexity of the classification process is O(N). Regardless of the size of the array, the algorithm must make one full pass through the array to determine how ordered/unordered the array is.
### 4. Performing this analysis prior to sorting has no impact on the overall time-complexity. Performing an **O(N)* task to classify, followed by an **O(N²)* task to sort the data ultimately results in an overall time complexity of O(N) + O(N²) = O(N²). However, choosing Insertion sort on a more ordered (small) dataset may provide a small advantage in operation count, regardless of Big-O complexity. Regardless, the classification algorithm provides linear time-complexity **O(N)* by comparing each adjacent pair only once. 

# Part D: Documentation and Analysis
## Threshold Definition: The adaptive sorting strategy I chose analyzes the initial order of the array by counting how many adjacent pairs are disordered. The program checks the values against each other and if the current element is greater than the next element, the pair is considered disordered.
### 1. 0 - 10% is considered Best/Nearly Sorted
### 2. 10% - 40% is considered Average/Partially Ordered
### 3. >40% is considered Worst/Highly Reverse-Ordered

## Threshold Justification/Algorithm Selection: 
### An array with 10% or fewer disordered pairs is relatively quick to sort with Insertion sort. The algorithm will perform closer to O(N) in this case as most elements are already partially sorted. As a result, the comparisons/swaps Insertion sort must make will be fewer.
### An array with 10% - 40% disordered pairs won't be able to take advantage of Insertion sort's efficiency as the Best/Nearly sorted array will. However, in the worst-case, Insertion sort is **O(N²)* which is the same as Selection sort. In this case, Insertion sort may provide some operational advantage on a case-by-case basis. 
### An array with more than 40% of its pairs out of order will likely not benefit from Insertion sort in any scenario. In this case, Selection sort provides a consistent result and Big-O time complexity, regardless of array size. Ultimately, using Selection sort beyond this threshold was more of a matter of consistency rather than trying to find the exact best-case time-complexity. 

## Time Complexity:
### Selection sort remains **O(N²)* regardless on the order of the array because it searches all unsorted elements until it finds the minimum value. Even when the array is already sorted, it performs a similar number of comparisons.
### Insertion sort can approach **O(N)* depending on the initial ordering of the data. If an array is mostly or completely pre-sorted, the elements are already in their correct position, requiring fewer comparisons/swaps. In relatively unsorted or reverse-ordered datasets, Insertion sort must complete up to N comparisons/swaps N times. N x N results in **O(N²)*.
### Even though both algorithms have the same overall worst-case time complexity, Insertion sort can vary depending on how ordered the data is prior to executing the algorithm. In nearly-ordered and even partially-ordered datasets, Insertion sort can edge out an advantage over Selection sort in actual operations required. Selection sort, on the other hand provides a consistent operational cost in partially ordered or reverse-ordered datasets.

# Final Analysis and Reflection
### Without sorting the array, it's still possible to determine how ordered or disordered the elements are if you perform the adjacent pair comparison. My program performs this check ```N - 1``` times, which adds extra work but doesn't impact the overall time-complexity of the program. In this case, there isn't a huge advantage to using the adaptive strategy but while writing this program, I could imagine a situation where hundreds of thousands or even millions of API calls were made in quick succession, relying on a program like this to efficiently sort a wide variety of datasets. In this case, the small advantage of choosing an adaptive sorting algorithm would begin to compound, resulting in a significant overall performance increase. Even though the overall Big-O complexity of each algorithm is the same, there is an advantage to choosing them adaptively.  

