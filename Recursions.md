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
| Step | Function Call | Value Printed | Next Action |
| :--- | :--- | :--- | :--- |
| 1 | `countdown(3)` | 3 | Call `countdown(2)` |
| 2 | `countdown(2)` | 2 | Call `countdown(1)` |
| 3 | `countdown(1)` | 1 | Call `countdown(0)` |
| 4 | `countdown(0)` | 0 | Return; the previous calls finish |

### The base case prevents negative numbers because when ```number``` reaches 0, the function prints 0 and returns. This prevents any additional recursive calls from ```number - 1```, this allows the countdown to stop at 0 instead of continuing into negative numbers. 

# Part 4: Reading Recursive Code: Factorial
### 1. What is the base case?
### When ```n == 1``` the function returns 1 without another recursive call.
### 2. What is the recursive case?
### When ```n > 1``` the function returns ```n * factorial(n - 1)```
### 3. How does ```n - 1``` move the function toward the base case?
### Every recursive call decreases ```n``` by 1 until it reaches 1, which is the base case.
### 4. What does ```factorial(1)``` return?
### It returns 1.
### 5. Use that result to determine ```factorial(2)```.
### ```factorial(2) = 2 * factorial(1) = 2 * 1 = 2```
### 6. Use the previous result to determine ```factorial(3)```.
### ```factorial(3) = 3 * factorial(2) = 3 * 2 = 6```
### 7. Determine ```factorial(4)```.
### ```factorial(4) = 4 * factorial(3) = 4 * 6 = 24```

# Part 5: Expanding Recursive Calls
## Expand ```factorial(4)``` until the base case is reached.
```
factorial(4)
= 4 * factorial(3)
= 4 * 3 * factorial(2)
= 4 * 3 * 2 * factorial(1)
= 4 * 3 * 2 * 1
```
### At what point does the recursive calling phase stop?
### The recursive calls stop at ```factorial(1)``` because it reaches the base case and returns 1 without calling the function again.

# Part 6: The Call Stack
## Order in which functions are called:
| Call Order | Function |
| :--- | :--- |
| 1 | `factorial(4)` |
| 2 | `factorial(3)` |
| 3 | `factorial(2)` |
| 4 | `factorial(1)` |

## Order in which functions complete:
| Completion Order | Function |
| :--- | :--- |
| 1 | `factorial(1)` |
| 2 | `factorial(2)` |
| 3 | `factorial(3)` |
| 4 | `factorial(4)` |

### Why is the completion order the reverse of the call order?
### The call stack follows LIFO, therefore the last function called is the first to finish. Each previous function must wait for the next recursive call's return before it can finish the calculation.

# Part 7: Passing Values Up the Call Stack
| Function | Expression | Returned Value |
| :--- | :--- | :--- |
| factorial(1) | `return 1` | 1 |
| factorial(2) | `2 * 1` | 2 |
| factorial(3) | `3 * 2` | 6 |
| factorial(4) | `4 * 6` | 24 |

### What does it mean for a value to be passed back up through the call stack?
### Each function returns the calculated value to the function calling it. This allows the previous function to finish the calculation and return its own value until the original call is complete.

# Part 8: Debugging Recursive Functions
## Example A
### 1. Does this function contain a base case?
### Yes, when ```n == 0``` the function returns without making another recursive call.
### 2. Will the base case ever be reached?
### No, starting with ```mystery(5)``` the value of ```n``` increases instead of decreasing toward 0.
### 3. What is wrong with the recursive step?
### The recursive call ```mystery(n + 1)``` moves away from the base case by increasing ```n``` each time.
### 4. How would you correct it?
### Change the recursive call to ```mystery(n - 1)```. This decreases ```n``` by 1 with each call until it reaches 0.

## Example B
### Why does this function eventually cause a stack overflow?
### There is no base case to stop the recursion. The function keeps calling ```mystery2(n - 1)``` without returning, so the call stack continues growing until it runs out of memory.

# Part 9: Write a Recursive Function
### What is the base case?
### When ```n == 1```, the function returns 1 without making another recursive call.
### What is the recursive case?
### When ```n > 1```, the function returns ```n + sumTo(n - 1)```.
### How does the recursive argument move toward the base case?
### Every call decreases ```n``` by 1 until reaching 1.

## Recursive function:
```C++
#include <iostream>
using namespace std;

int sumTo(int n) {
    if (n == 1) {
        return 1;
    }

    return n + sumTo(n - 1);
}

int main() {
    cout << sumTo(5) << endl;
    return 0;
}
```

## Trace of ```sumTo(4)```:
### Recursive calls:
```
sumTo(4)
= 4 + sumTo(3)
= 4 + 3 + sumTo(2)
= 4 + 3 + 2 + sumTo(1)
= 4 + 3 + 2 + 1
```
### Returned values:
```
sumTo(1) = 1
sumTo(2) = 2 + 1 = 3
sumTo(3) = 3 + 3 = 6
sumTo(4) = 4 + 6 = 10
```

# Part 10: Recursion vs. Iteration
| Question | Response |
| :--- | :--- |
| Which version is easier for you to read? | The loop-based version because the countdown is straightforward with a for loop. |
| Which version uses the call stack? | Both use the call stack, but recursion adds another function call to the stack each time. |
| Is recursion automatically more efficient than iteration? | No, recursion can use more memory because each call adds another stack frame. |
| Why might a loop still be preferable for a simple countdown? | A loop is simpler and doesn't need additional recursive function calls. |

# Part 11: Recursive Structure: Filesystem Traversal
### 1. Why is recursion a natural approach to this problem?
### Each directory can contain more directories. Recursion allows the same function to process each level without needing to know how many levels there are.
### 2. What could serve as the base case when processing a directory?
### When a directory has no subdirectories, there are no additional recursive calls to make.
### 3. What would the recursive case do?
### The function would go through each subdirectory and call itself to process that directory.
### 4. Why is it difficult to know in advance how many levels of nesting may exist?
### Different directories can contain different numbers of nested subdirectories, so the number of levels isn't always known ahead of time.

## Pseudocode:
```text
FUNCTION traverse(directory)
    DISPLAY directory
    FOR each subdirectory in directory
        CALL traverse(subdirectory)
    END FOR
END FUNCTION
```

# Analysis and Reflection
### 1. Why does every useful recursive function need a stopping condition?
### Without a stopping condition, the function would continue calling itself instead of finishing.
### 2. Why is having a base case not enough if the recursive step never reaches it?
### The recursive step needs to move toward the base case. If it doesn't, the function will continue making recursive calls without reaching a stopping condition.
### 3. What happens during the recursive calling phase?
### Each function calls itself again with a new argument. The unfinished calls are stored on the call stack until the base case is reached.
### 4. What happens during the unwinding phase?
### Once the base case returns, each previous function continues and finishes in reverse order.
### 5. Why does the call stack behave naturally as a LIFO structure for recursion?
### The most recent function call must finish before the function that called it can continue. This means the last call added to the stack is the first one to finish.
### 6. Why can infinite recursion cause stack overflow?
### Every recursive call uses more space on the call stack. Without a stopping condition, the stack can run out of memory and overflow.
### 7. Why is recursion not automatically preferable to iteration?
### Recursion uses additional function calls and stack memory. A loop can be simpler and more efficient in certain problems (like a countdown).
### 8. Why is recursion useful for problems containing an unknown number of nested levels?
### Each recursive call can process levels of a problem without needing to know the total amount of levels ahead of time.
