# Week 3/4 Adaptive Sorting:

# Part A: Adaptive Sorting Selection 
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
## The function: countAdjacentDisorder compares ```C++ currentIndex``` and ```C++ currentIndex + 1``` to determine how many pairs are out of order. When a pair is detected, ```C++ disorderCount``` increments.
