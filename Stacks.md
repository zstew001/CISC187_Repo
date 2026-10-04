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



