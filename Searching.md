# Question 1:
## Consider the following ordered array: [2, 4, 6, 8, 10, 12, 13], how many comparisons would it take to find the number 8 using linear search?
## It would take **4 comparisons** to find 8 using a linear search algorithm. Linear search would begin by comparing the first array element to the target value (8). Since 2 != 8, the linear search algorithm continues checking each element until finding 8 which is the 4th array element.

# Question 2: 
## Using the same ordered array: [2, 4, 6, 8, 10, 12, 13], how many comparisons would it take to find the number 8 using binary search?
## It would take **1 comparison** to find 8 using a binary search algorithm. Binary search starts with the middle element and does a > = < comparison to determine where in the array it should search (halving the searched array size on each comparison). Since 8 is the middle element and 8 = 8, a binary search algorithm would terminate after the first comparison. 

# Question 3: 
## What is the maximum number of comparisons required to perform a binary search on a sorted array containing 100,000 elements?
## Binary search has a worst-case time complexity of log₂N. Since N = 100,000 in this case, log₂(100,000) = 16.6096. Since you can't have a fractional step, it would take at most **17 steps**. Binary search works by checking the middle element and eliminating roughly half of the remaining elements after each comparison step.

# Question 4: 
## Write a C++ program that implements both linear search and binary search using a dataset containing 100,000 elements.
## Your program should:
###   **Search for the same target value using both algorithms.**
###   **Record the number of element-to-target comparisons performed by linear search.**
###   **Record the number of element-to-target comparisons performed by binary search.**
###   **Report whether the target was found.**
###   ***Display the number of comparisons performed by each algorithm.***
###   **Test your program using different target values, including:**
###      **A value near the beginning of the dataset.**
###      **A value near the end of the dataset.**
###      **A value that does not exist in the dataset.**
# Question 4 Code:
```C++
#include <iostream>
#include <vector>

using namespace std;
// Function for Linear Search algorithm
bool linearSearch(vector<int> dataSet, int target, int& comparisons){
    comparisons = 0;

    for (int i = 0; i < dataSet.size(); i++)
    {
        comparisons++;

        if (dataSet[i] == target)
        {
            return true;
        }
    }

    return false;

}
// Function for Binary Search algorithm
bool binarySearch(vector<int> dataSet, int target, int& comparisons){
    comparisons = 0;

    int low = 0;
    int high = dataSet.size() - 1;

    while (low <= high){
        int mid = (low + high) / 2;

        comparisons++;

        if (dataSet[mid] == target){
            return true;
        }
        else if (dataSet[mid] < target){
            low = mid + 1;
        }
        else{
            high = mid - 1;
        }
    }

    return false;

}

int main()
{
    vector<int> dataSet;

    
    for (int i = 1; i <= 100000; i++){
        dataSet.push_back(i);
    }

    int targets[3] = { 8, 98000, 100001 }; // 3 target values: one near beginning of dataSet, end of dataSet and not contained within dataSet 

    for (int i = 0; i < 3; i++){
        int target = targets[i];
        int linearComparisons;
        int binaryComparisons;

        bool foundLinear = linearSearch(
            dataSet, target, linearComparisons);

        bool foundBinary = binarySearch(
            dataSet, target, binaryComparisons);

        cout << "Target value: " << target << endl;

        cout << "Linear search:" << endl;

        if (foundLinear){
            cout << "Target found" << endl;
        }
        else{
            cout << "Target not found" << endl;
        }

        cout << "Number of comparisons: "
            << linearComparisons << endl;

        cout << endl;

        cout << "Binary search:" << endl;

        if (foundBinary){
            cout << "Target found" << endl;
        }
        else{
            cout << "Target not found" << endl;
        }

        cout << "Number of comparisons: "
            << binaryComparisons << endl;

        cout << endl;
    }

    return 0;

}
```
