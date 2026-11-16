# Part 1: Recognizing Recursion
### Consider the following function: 
```
void example() {
    example();
}
```
### 1. Why is this function recursive?
### The function is recursive because ```example()``` calls itself inside its own function.
### 2. Does this function contain a base case?
### No, there is no base case to stop the recursion.
### 3. What will happen if ```example()``` is called?
### The function will call itself until the stack runs out of memory, which will result in a stack overflow.
### 4. What happens to the call stack as the function continues to call itself?
### Every call allocates more memory from the stack. None of the calls finish and the stack continues growing until it overflows. 

# Part 2: From Loop to Recursion
## Iterative function:
```C++
#include <iostream>
using namespace std;

void countdown(int num) {
    for (int i = num; i >= 0; i--) {
        cout << i << endl;
    }
}

int main() {
    countdown(5);
    return 0;
}
```
### What is the base case?
### When ```num == 0```, the function prints 0 and returns without making another function call.
### What is the recursive case?
### When ```num > 0```, the function calls ```countdown(num - 1)```
### How does each call move toward the base case?
### Every call decreases ```num``` by 1 until reaching 0.

## Recursive function:
```C++
#include <iostream>
using namespace std;

void countdown(int number) {
    cout << number << endl;

    if (number == 0) {
        return;
    }

    countdown(number - 1);
}

int main() {
    countdown(5);
    return 0;
}
```

# Part 3: Trace the Countdown


