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

# Part 2: Implement an Array-Based Stack
```
#include <iostream>

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
        return;
    }

    topIndex++;
    data[topIndex] = value;
}

int Stack::pop() {
    if (empty()) {
        return -1;
    }

    int value = data[topIndex];
    topIndex--;
    return value;
}

int Stack::top() const {
    if (empty()) {
        return -1;
    }

    return data[topIndex];
}

int main() {
    Stack myStack;

    myStack.push(10);
    myStack.push(20);
    myStack.push(30);

    cout << "Top element: " << myStack.top() << endl;
    cout << "Stack size: " << myStack.size() << endl;

    cout << "Popped: " << myStack.pop() << endl;

    cout << "Top element: " << myStack.top() << endl;
    cout << "Stack size: " << myStack.size() << endl;

    return 0;
}
```
