# Question 1: 
## Explain how to create an array of 100 elements. You can choose any data type of your choice.
# Question 2: 
## What will be the size of each element of an array.

```C++
#include <iostream>
using namespace std;

int main() {


	int sizeOfOneElement = 0;
	int arr[100]; // This line creates an integer array with 100 elements, indexed from 0 to 99.

	sizeOfOneElement = sizeof(arr[0]);

	cout << "Array element size: " << sizeOfOneElement << endl; // Print statement to show element size for an int which is 4 bytes.

	return 0;

}
```

# Question 3:
## For an array containing 100 elements, provide the number of steps the following operations would take:
### a. Reading: Takes 1 step. i.e. arr[0] directly reads the array element at index 0. 
### b. Searching for a value not contained within the array: Takes 100 steps. If the value is not contained in the array, every array element will be checked. 
### c. Insertion at the beginning of the array: Takes 101 steps. The array would have to shift all array elements to N + 1. i.e. arr[1] becomes arr[2]. Once all array elements are increased by 1, the new element is inserted at arr[0].
### d. Insertion at the end of the array: Takes 1 step. The compiler increases the size of the array by 1 then assigns the value to the new position. 
### e. Deletion at the beginning of the array: Takes 100 steps. The beginning element is deleted, then every other array element is shifted to N - 1. i.e. arr[1] becomes arr[0].
### f. Deletion at the end of the array: Takes 1 step. Either you can overwrite the value of the last array element, or decrease the size of the array by 1 element. 

# Question 4:
## Normally the search operation in an array looks for the first instance of a given value. But sometimes we may want to look for every instance of a given value. 
## For example, say we want to count how many times the value “apple” is found inside an array. How many steps would it take to find all the “apples”? Give your answer in terms of N.
### For an array of size N, we'd have to perform N operations to find all occurrences of "apple", as every array element has to be checked to see if it matches the value of "apple".

# Question 5: 
## Research how to find the memory address of an array. You can use any programming language of your choice.
```C++
#include <iostream>
using namespace std;

int main() {


    int arr[100];

    cout << "Array address: " << arr << endl; // Calling the array name checks the memory address of the array.
    cout << "Address of first array element: " << &arr[0] << endl; // &arr uses the Address-of operator to check the memory address of the chosen element.

    return 0;
}
```
