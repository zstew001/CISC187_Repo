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
###   **Display the number of comparisons performed by each algorithm.**
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
bool linearSearch(vector<int> dataSet, int target, int& comparisons) {
    comparisons = 0;

    for (int i = 0; i < dataSet.size(); i++) {
        comparisons++;

        if (dataSet[i] == target) {
            return true;
        }
    }

    return false;

}
// Function for Binary Search algorithm
bool binarySearch(vector<int> dataSet, int target, int& comparisons) {
    comparisons = 0;

    int low = 0;
    int high = dataSet.size() - 1;

    while (low <= high) {
        int mid = (low + high) / 2;

        comparisons++;

        if (dataSet[mid] == target) {
            return true;
        }
        else if (dataSet[mid] < target) {
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }

    return false;

}

int main() {
    vector<int> dataSet;

    
    for (int i = 1; i <= 100000; i++) {
        dataSet.push_back(i);
    }

    int targets[3] = { 8, 98000, 100001 }; // 3 target values: one near beginning of dataSet, end of dataSet and not contained within dataSet 

    for (int i = 0; i < 3; i++) {
        int target = targets[i];
        int linearComparisons;
        int binaryComparisons;

        bool foundLinear = linearSearch(dataSet, target, linearComparisons);

        bool foundBinary = binarySearch(dataSet, target, binaryComparisons);

        cout << "Target value: " << target << endl;

        cout << "Linear search:" << endl;

        if (foundLinear) {
            cout << "Target found" << endl;
        }
        else {
            cout << "Target not found" << endl;
        }

        cout << "Number of comparisons: "
            << linearComparisons << endl;

        cout << endl;

        cout << "Binary search:" << endl;

        if (foundBinary) {
            cout << "Target found" << endl;
        }
        else {
            cout << "Target not found" << endl;
        }

        cout << "Number of comparisons: "
            << binaryComparisons << endl;

        cout << endl;
    }

    return 0;

}
```
# Question 4 Analysis:
## 1. Linear search has a worst-case time complexity of O(N) because each element is checked once at a time. For example, if an element isn't in the data set, linear search must check each element one-by-one until reaching the end of the data set. The size of the remaining search space decreases by one after each check. Therefore, if the element is the last element in the data set to be checked or isn't contained in the data set, a 100,000 element data set would need to perform 100,000 checks to determine if the target value is present. 

## 2. Binary search has a worst-case time complexity of O(Log N) because each comparison cuts the portion of the data set that needs to be searched in half. Binary search begins by checking the middle element and then determining if the target value is greater than, equal to, or less than the middle element. If the target element is smaller than the middle element, the upper half of the data set is eliminated and vice-versa. Therefore, mathematically, binary search only needs to perform log₂N comparisons. However, it's important to note that binary search only works on a sorted data set.

## 3. Binary search requires a sorted data set because otherwise, it will have no way to ensure that the greater than, equal to, or less than comparison can be used to determine which half should be eliminated. For example, if the data set isn't sorted, binary search will miss potential target value matches each time it eliminates a portion of the data set to be checked. Linear search, on the other hand, is much simpler and checks each value one-by-one. This means that while it can be less efficient in a sorted data set, it guarantees that each value will be checked and the target value will be found if it exists. 

# Question 5: Randomized Search
## Design a randomized search algorithm that searches for a given key by randomly selecting indices without repetition.
### Use a dataset containing 100,000 distinct elements stored in a C++ <vector>.
### Each element may be eliminated at most once during a single search.

## Pseudocode:
```C++
function random_search(A, n, T)

checked[0 to n - 1] := false

count := 0
comparisons := 0

while count < n
    i := random integer from 0 to n - 1

    if checked[i] = false
        checked[i] := true
        count := count + 1
        comparisons := comparisons + 1

        if A[i] = T
            return i

return unsuccessful
```
## Complexity Analysis: ***Hopefully this pseudocode is in an acceptable format. I tried to write it in a way that matches the style/syntax of your pseudocode in the lecture slide.***

### Best-case: O(1) - The target value could be found on the first randomly selected index. Very unlikely in a dataset containing 100,000 elements but this is the best case time-complexity.
### Average-case: O(N/2) - Statistically, the average case will end up somewhere around 50,000 checks in a 100,000 element data set. However, this will vary pretty significantly each time a target value is searched for.
### Worst-case: O(N) - The target value could be the last randomly selected index. This means that all 100,000 elements will be checked. Similarly, if the target value doesn't exist, the algorithm will check every index before returning unsuccessful.

## Implementation: 
```C++
#include <vector>
#include <random>
#include <iostream>

using namespace std;

// Function for random search algorithm
bool randomSearch(vector<int> dataSet, int target, int& comparisons) {
    vector<bool> checked(dataSet.size(), false);
    int count = 0;
    comparisons = 0;

    random_device random; // I hope this is right - I had to look it up. I never used this in CISC 192 but you didn't include <cstdlib> in your list of headers.
    mt19937 gen(random());
    uniform_int_distribution<> dis(0, dataSet.size() - 1); 

    while (count < dataSet.size()) {
        int i = dis(gen);

        if (checked[i] == false) {
            checked[i] = true;
            count++;

            comparisons++;

            if (dataSet[i] == target) {
                return true;
            }
        }
    }

    return false;
}

int main() {
    vector<int> dataSet;

    for (int i = 1; i <= 100000; i++) {
        dataSet.push_back(i);
    }

    int target = 100;
    int randomComparisons;

    bool found = randomSearch(dataSet, target, randomComparisons);

    if (found) {
        cout << "Target found" << endl;
    }
    else {
        cout << "Target not found" << endl;
    }

    cout << "Number of comparisons: "
        << randomComparisons << endl;

    return 0;
}
```
## Comparison: 
### **Linear search** has a best-case time complexity of O(1) and average and worst-case time complexity of O(N). The advantage of linear search is that the data doesn't need to be sorted, making insertions easier to manage. However, in large datasets linear search can be slower and less efficient. **Binary search** on the other hand has a best-case time complexity of O(1) and average and worst-case time complexity of O(log₂N). Binary search requires the data to be sorted but is significantly more time-efficient as the size of the data set increases. However, maintaining a sorted data set can make inserting new elements more costly from a memory management perspective. **Randomized search** has a best-case time complexity of O(1) and average and worst-case time complexity of O(N) (the statistical relevance of average case being (N/2) isn't a reliable advantage over linear search). While random search doesn't require a sorted data set, the algorithm does have to track previously checked indices to prevent itself from repeatedly comparing an already checked value. Randomly examining elements doesn't automatically produce better asymptotic time complexity than linear search because it still checks indexes one-by-one. So, unlike binary search, it doesn't gain improved efficiency by eliminating portions of the dataset. Overall, binary search has a clear advantage when searching sorted data sets but linear and randomized search seem best for smaller or unsorted data samples. One thing to consider is that there may be situations where randomly searching a data set may be advantageous if a user wanted to avoid checking elements in a predictable order.   
