# Week 6 Assignment 1: Stacks
# Part 1: Trace Stack Operations
##     Operation	       Value Returned	  Stack After Operation    Top Element    Stack Size
### ```push(10)```	           —			         [10]                    10              1
### ```push(20)```	           —			         [10, 20]                20              2
### ```push(30)```	           —			         [10, 20, 30]            30              3
### ```pop()```				         30              [10, 20]                20              2
### ```push(40)```	           —			         [10, 20, 40]            40              3
### ```push(50)```	           —			         [10, 20, 40, 50]        50              4
### ```pop()```				         50              [10, 20, 40]            40              3
### ```push(60)```	           —               [10, 20, 40, 60]        60              4
## Analysis:
### 1. The final top element is 60.
### 2. The final stack size is 4.
### 3. The remaining elements would be removed in the order: [60, 40, 20, 10].
### 4. LIFO (Last-In, First-Out) is shown in this example because the last element added would be the first element removed.

# Part 2-7: Implement an Array-Based Stack
```C++
#include <iostream>
#include <stdexcept>

using namespace std;

class Stack {
private:
    static const int CAPACITY = 10;

    int data[CAPACITY];
    int topIndex;

public:
    Stack();

    bool empty() const;
    bool full() const;
    int size() const;

    void push(int value);
    int pop();
    int top() const;
};

Stack::Stack() {
    topIndex = -1;
}

bool Stack::empty() const {
    return topIndex == -1;
}

bool Stack::full() const {
    return topIndex == CAPACITY - 1;
}

int Stack::size() const {
    return topIndex + 1;
}

void Stack::push(int value) {
    if (full()) {
        throw overflow_error("Stack overflow");
    }

    ++topIndex;
    data[topIndex] = value;
}

int Stack::pop() {
    if (empty()) {
        throw underflow_error("Stack underflow");
    }

    int value = data[topIndex];
    --topIndex;
    return value;
}

int Stack::top() const {
    if (empty()) {
        throw underflow_error("Stack underflow");
    }

    return data[topIndex];
}

int main() {
    Stack myStack;

    cout << "Is the stack empty? " << myStack.empty() << endl;

    myStack.push(10);
    myStack.push(20);
    myStack.push(30);
    myStack.push(40);
    myStack.push(50);

    cout << "Stack size: " << myStack.size() << endl;
    cout << "Top: " << myStack.top() << endl;

    cout << "Pop: " << myStack.pop() << endl;
    cout << "Pop: " << myStack.pop() << endl;

    cout << "New top: " << myStack.top() << endl;
    cout << "New size: " << myStack.size() << endl;

    cout << "Pop: " << myStack.pop() << endl;
    cout << "Pop: " << myStack.pop() << endl;
    cout << "Pop: " << myStack.pop() << endl;

    cout << "Is the stack empty? " << myStack.empty() << endl;

    try {
        myStack.pop();
    }
    catch (const underflow_error& e) {
        cout << e.what() << endl;
    }

    myStack.push(10);
    myStack.push(20);
    myStack.push(30);
    myStack.push(40);
    myStack.push(50);
    myStack.push(60);
    myStack.push(70);
    myStack.push(80);
    myStack.push(90);
    myStack.push(100);

    cout << "Stack size: " << myStack.size() << endl;
    cout << "Is the stack full? " << myStack.full() << endl;

    try {
        myStack.push(110);
    }
    catch (const overflow_error& e) {
        cout << e.what() << endl;
    }

    return 0;
}
```
# Part 3 Analysis:
### 1. Why should topIndex initially be -1 rather than 0?
### - The index starts at 0. If topIndex is 0, the stack contains 1 element, so -1 represents an empty stack.
### 2. Why is the stack size topIndex + 1?
### - The first element is stored at index 0. For example, topIndex = 0 contains 1 element. So, in order to accurately represent the stack size, we have to take into account that the number of elements is + 1 compared to the index of the top element. 
### 3. What value of topIndex indicates that the stack is full?
### - topIndex == CAPACITY - 1; and CAPACITY = 10, so when topIndex = 9, the stack is full.

# Part 4 Analysis:
### 1. Explain why writing beyond: ```data[CAPACITY - 1]``` would be incorrect.
### - CAPACITY = 10, so the valid indexes are 0 - 9. If you write beyond the array bounds, you can overwrite other memory. 

# Part 5 Analysis:
### 1. Explain why accessing ```data[topIndex]``` when ```topIndex = -1``` is invalid.
### - When topIndex = -1, the stack is empty. If the stack is empty, there is no valid element to access.

# Part 6 Analysis:
### Explain the difference between: ```top()``` and ```pop()```.
### - top() returns the top element in the stack without removing it (inspects). pop() returns then removes the top element. 

# Part 8: Complexity Analysis
### push() is O(1) time complexity. It only changes topIndex and stores a new value. The number of elements in the stack doesn't change the amount of work.
### pop() is O(1) time complexity. It directly checks topIndex to find and remove the top value. As a result, it doesn't interact with any other elements in the stack and doesn't change the amount of work.
### top() is O(1) time complexity. It inspects topIndex to find the element at the top of the stack. This also isn't affected by other elements in the stack and doesn't change the amount of work.
### empty() is O(1) time complexity. It simply checks if topIndex == -1. This doesn't interact with other elements in the stack and as a result, doesn't change the amount of work.
### full() is O(1) time complexity. It checks if topIndex == CAPACITY - 1. So, no other elements are examined and no additonal work is required regardless of # of elements in the stack.
### size() is O(1) time complexity. This returns topIndex + 1, so it directly returns the size of the stack without needing to count elements. Therefore, no additional work is required regardless of the stack size.

### One Million Elements: Even in a stack containing 1,000,000 elements, pop() could be used to remove the top element without examining any of the other 999,999 elements. This is why topIndex is so important. Without topIndex, pop() wouldn't be able to immediately locate and remove the top element. You'd have to use a search algorithm to find the top element. As we've seen, search algorithms aren't O(1) so you'd lose a lot of efficiency. 

# Part 9: Stack Correctness
### 1. Following the four push() operations, four consecutive pop() operations would return: 20, 15, 10, 5.
### 2. The general relationship is ```push(x1), push(x2), ..., push(xN)``` then four consecutive pop()'s would return ```xN, xN-1, ..., x2, x1``` (Last-In, First-Out). This property can be used to test whether your stack implementation is correct because it shows that push() is correctly placing values at the top of the stack and pop() is correctly returning then removing them. If the order doesn't follow this general pattern, it's fairly likely a mistake was made with the programmer's stack implementation. 

# Part 11: Implement the Balanced-Delimiter Algorithm
```C++
#include <iostream>
#include <stack>
#include <string>

using namespace std;

bool balanced(const string& expression) {
    stack<char> delimiters;

    for (char character : expression) {
        if (character == '(' || character == '[' || character == '{') {
            delimiters.push(character);
        }
        else if (character == ')' || character == ']' || character == '}') {
            if (delimiters.empty()) {
                return false;
            }

            char opening = delimiters.top();
            delimiters.pop();

            if ((character == ')' && opening != '(') ||
                (character == ']' && opening != '[') ||
                (character == '}' && opening != '{')) {
                return false;
            }
        }
    }

    return delimiters.empty();
}

int main() {
    string expressions[] = {
        "{(a+b)*[c-d]}",
        "{(a+b]*c}",
        "((a+b))",
        "((a+b)",
        "[a+b]",
        "{[()]}",
        "{[(])}"
    };

    for (string expression : expressions) {
        cout << "Expression: " << expression << endl;
        cout << "Balanced: " << (balanced(expression) ? "true" : "false") << endl;
        cout << endl;
    }

    return 0;
}
```
# Part 12: Analyze Delimiter Matching
### 1. Explain how many times each character is examined.
### - Each character is only examined once as the algorithm moves from left to right.
### 2. Explain why each delimiter is pushed or popped at most once.
### - An opening delimiter is pushed once when the algorithm encounters it. It is also only popped once when the matching closing delimiter is found.
### 3. Explain the time complexity of the algorithm.
### - As push(), pop(), and top() are all O(1) and every character must be examined once, which is O(N), their combined time complexity is O(N).
### 4. Explain the worst-case space complexity.
### - I think the worst-case space complexity would be O(N). For example, if there was N opening delimiters and no closing delimiters, all of the opening delimiters would remain in the stack, requiring N spaces. 

# Part 13: Stack Applications
## Scenario A: Undo
### This scenario follows LIFO behavior. In this case a stack would be appropriate because the most recent operation needs to be undone first. I.E. Google docs follows this logic with their undo function. 

## Scenario B: Function Calls
### This scenario also follows LIFO behavior. A stack is appropriate here because the most recently called function would need to be removed from the stack then the new top of the stack could be called to return to the previous function.

## Scenario C: Browser Back Navigation
### This follows LIFO behavior and could use a stack to recall the most recently visited previous browser page. The page could be stored at the top of the stack and called quickly then removed with pop() after navigating back.

## Scenario D: Depth-First Search
### Exploring the most recently discovered path before returning to earlier alternatives follows LIFO behavior which is appropriate for a stack. For example, if you had File A, which had subfiles B and C, and you wanted to perform a DFS, you'd want to search C, then B, then A. A stack would perform correctly here because C would be placed on the stack last and after it was searched could be removed, then B, then A.

## Scenario E: Customer Service Line
### A customer service line exhibits FIFO behavior and wouldn't be appropriate for a stack. A queue would be more appropriate (which makes sense because it's synonymous with "line"). With a customer service line, you'd generally want to service customers in the order they arrived. A stack would keep servicing the most recent arrival. 





