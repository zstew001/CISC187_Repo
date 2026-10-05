
# Week 7 Assignment 1: Queues

# Part 1: Trace Queue Operations
## Operation Value Returned Logical Queue After Operation Front Size
### `enqueue(10)` — [10] 10 1
### `enqueue(20)` — [10, 20] 10 2
### `enqueue(30)` — [10, 20, 30] 10 3
### `dequeue()` 10 [20, 30] 20 2
### `enqueue(40)` — [20, 30, 40] 20 3
### `enqueue(50)` — [20, 30, 40, 50] 20 4
### `dequeue()` 20 [30, 40, 50] 30 3
### `enqueue(60)` — [30, 40, 50, 60] 30 4

## Analysis:
### 1. The final front element is 30.
### 2. The final queue size is 4.
### 3. The remaining elements would be removed in the order: [30, 40, 50, 60].
### 4. This demonstrates FIFO (First-In, First-Out) because the first element added is the first element removed.

# Part 2: Why Not Shift the Array
### 1. If the queue contains N elements, approximately N - 1 elements may need to move during one `dequeue()` because the elements after the front have to shift left.
### 2. The Big-O complexity is O(N) because the amount of shifting depends on the number of elements in the queue.
### 3. Repeatedly removing all N elements can require O(N²) total work because the first dequeue may move about N elements, the next moves about N - 1, and so on. This gives N + (N - 1) + ... + 1, which is O(N²).
### 4. Advancing the front index is better because it moves directly to the next element without moving the others. This makes `dequeue()` O(1) instead of O(N).

# Part 3-7: Implement a Circular Queue
```C++
#include <iostream>
#include <stdexcept>

using namespace std;

class Queue {
private:
    static const int CAPACITY = 10;

    int data[CAPACITY];
    int frontIndex;
    int rearIndex;
    int count;

public:
    Queue();

    bool empty() const;
    bool full() const;
    int size() const;

    void enqueue(int value);
    int dequeue();
    int front() const;
};

Queue::Queue() {
    frontIndex = 0;
    rearIndex = 0;
    count = 0;
}

bool Queue::empty() const {
    return count == 0;
}

bool Queue::full() const {
    return count == CAPACITY;
}

int Queue::size() const {
    return count;
}

void Queue::enqueue(int value) {
    if (full()) {
        throw overflow_error("Queue overflow");
    }

    data[rearIndex] = value;
    rearIndex = (rearIndex + 1) % CAPACITY;
    ++count;
}

int Queue::dequeue() {
    if (empty()) {
        throw underflow_error("Queue underflow");
    }

    int value = data[frontIndex];
    frontIndex = (frontIndex + 1) % CAPACITY;
    --count;
    return value;
}

int Queue::front() const {
    if (empty()) {
        throw underflow_error("Queue underflow");
    }

    return data[frontIndex];
}

int main() {
    Queue myQueue;

    cout << "Is the queue empty? " << myQueue.empty() << endl;

    myQueue.enqueue(10);
    myQueue.enqueue(20);
    myQueue.enqueue(30);
    myQueue.enqueue(40);
    myQueue.enqueue(50);

    cout << "Queue size: " << myQueue.size() << endl;
    cout << "Front: " << myQueue.front() << endl;

    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;

    cout << "New front: " << myQueue.front() << endl;
    cout << "New size: " << myQueue.size() << endl;

    myQueue.enqueue(60);
    myQueue.enqueue(70);
    myQueue.enqueue(80);
    myQueue.enqueue(90);
    myQueue.enqueue(100);

    cout << "Queue size: " << myQueue.size() << endl;
    cout << "Is the queue full? " << myQueue.full() << endl;

    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;
    cout << "Dequeue: " << myQueue.dequeue() << endl;

    cout << "Is the queue empty? " << myQueue.empty() << endl;

    try {
        myQueue.dequeue();
    }
    catch (const underflow_error& e) {
        cout << e.what() << endl;
    }

    myQueue.enqueue(10);
    myQueue.enqueue(20);
    myQueue.enqueue(30);
    myQueue.enqueue(40);
    myQueue.enqueue(50);
    myQueue.enqueue(60);
    myQueue.enqueue(70);
    myQueue.enqueue(80);
    myQueue.enqueue(90);
    myQueue.enqueue(100);

    cout << "Queue size: " << myQueue.size() << endl;
    cout << "Is the queue full? " << myQueue.full() << endl;

    try {
        myQueue.enqueue(110);
    }
    catch (const overflow_error& e) {
        cout << e.what() << endl;
    }

    return 0;
}
```
# Part 4 Analysis:
### 1. Why does `count == 0` represent an empty queue?
### - `count` tracks how many elements are in the queue. If count is 0, there are no elements, so the queue is empty.
### 2. Why does `count == CAPACITY` represent a full queue?
### - The queue can hold a maximum of CAPACITY elements. When count reaches CAPACITY, all available positions are being used.
### 3. Why can `frontIndex == rearIndex` represent either an empty or full circular queue in this design?
### - Since both indexes wrap around the array, they can eventually reach the same position. When the queue is empty, both start at 0. They can also become equal after the queue has wrapped around while being full.
### 4. How does maintaining `count` remove this ambiguity?
### - `count` tells us how many elements are actually stored. If the indexes are equal and count is 0, the queue is empty. If count is CAPACITY, the queue is full.

# Part 5 Analysis:
### 1. Explain why `++rearIndex;` by itself is not sufficient for a circular queue.
### - `rearIndex` can only contain indexes from 0 to CAPACITY - 1. If we only use `++rearIndex`, it will eventually become CAPACITY and go outside the array. `(rearIndex + 1) % CAPACITY` wraps the index back to 0 after the last position.

# Part 6 Analysis:
### 1. Explain why `dequeue()` =should advance `frontIndex` instead of shifting all remaining elements.
### - Advancing `frontIndex` moves directly to the next element without moving the others. This makes `dequeue()` O(1). Shifting the remaining elements would make it O(N).

# Part 7 Analysis:
### Explain the difference between: `front()` and `dequeue()`.
### - `front()` returns the oldest element without removing it. `dequeue()` returns the oldest element and removes it from the queue. Therefore, `front()` does not change the queue, while `dequeue()` does.

# Part 8: Demonstrate Circular Wraparound
