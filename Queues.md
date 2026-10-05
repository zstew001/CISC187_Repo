
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
## Operation Front Index Rear Index Count Logical Queue
### Initial 0 0 0 []
### `enqueue(10)` 0 1 1 [10]
### `enqueue(20)` 0 2 2 [10, 20]
### `enqueue(30)` 0 3 3 [10, 20, 30]
### `enqueue(40)` 0 4 4 [10, 20, 30, 40]
### `enqueue(50)` 0 0 5 [10, 20, 30, 40, 50]
### `dequeue()` 1 0 4 [20, 30, 40, 50]
### `dequeue()` 2 0 3 [30, 40, 50]
### `enqueue(60)` 2 1 4 [30, 40, 50, 60]

### Analysis:
### 1. When did wraparound occur?
### - Wraparound occurred when `rearIndex` went from index 4 back to index 0. It happened again when `enqueue(60)` moved the rear index from 0 to 1, reusing the position where 10 was stored.
### 2. Why is the next position after the last physical array index index 0?
### - The modulo operation causes this. When `rearIndex` is 4, `(4 + 1) % 5` equals 0, sending the index back to the beginning.
### 3. Why can physical array order differ from logical queue order?
### - The logical order starts at `frontIndex` and continues around the array. In this example, 60 is physically at index 0 but logically comes after 50 because the front is at index 2.
### 4. How does modular arithmetic make this possible?
### - `(index + 1) % CAPACITY` keeps the index within the valid range and allows it to wrap from the last position back to 0. This lets the queue reuse positions without moving elements.

# Part 9: Logical Position vs. Physical Position
## Logical Position Calculation Physical Index
### 0 `(6 + 0) % 8` 6
### 1 `(6 + 1) % 8` 7
### 2 `(6 + 2) % 8` 0
### 3 `(6 + 3) % 8` 1

### Analysis:
### - This allows the logical queue to cross the physical end of the array because the modulo operation returns to 0 once the calculation reaches 8. The logical positions are stored at indexes 6, 7, 0, and 1. No elements have to be moved because the front index and modulo operation determine their positions.

# Part 11: Complexity Analysis
### `enqueue()` is O(1). It checks if the queue is full, stores the value, advances `rearIndex`, and increases count. None of these operations depend on the queue size.
### `dequeue()` is O(1). It accesses the element at `frontIndex`, advances the index, decreases count, and returns the value. It does not move any other elements.
### `front()` is O(1) because it directly accesses the element at `frontIndex`.
### `empty()` is O(1) because it only checks whether count is 0.
### `full()` is O(1) because it only checks whether count equals CAPACITY.
### `size()` is O(1) because the current number of elements is already stored in count.
### Circular vs. Shifting Queue
### Implementation A: Every dequeue shifts the remaining elements.
### - One `dequeue()` is O(N) because the remaining elements may all need to be shifted left.
### Implementation B: Dequeue advances `frontIndex` using modulo.
### - One `dequeue()` is O(1) because it only accesses the front element, advances the index, and decreases count.
### Removing all N elements from Implementation A is O(N²) because each dequeue shifts a decreasing number of elements. The total work is approximately N + (N - 1) + ... + 1.
### Removing all N elements from Implementation B is O(N) because each dequeue is O(1), and there are N dequeue operations.

# Part 12: FIFO Correctness
### 1. Following the four enqueue() operations, four consecutive dequeue() operations would return: 5, 10, 15, 20.
### 2. In general, `enqueue(x1), enqueue(x2), ..., enqueue(xN)` followed by consecutive dequeue() operations should return `x1, x2, ..., xN`. This can be used to test the queue because it verifies that values are inserted at the rear and removed from the front in the correct order. If the values come out in a different order, there is likely a problem with the implementation.

# Part 13: Queue Applications
## Scenario A: Print Server
### This scenario follows FIFO behavior. A queue is appropriate because print jobs should be processed in the order they arrive. The first job added should be the first one processed.
## Scenario B: Server Requests
### This scenario follows FIFO behavior. A queue is appropriate because requests should generally be handled in the order they arrive. This keeps newer requests from being processed before ones that have already been waiting.
## Scenario C: Undo
### This scenario follows LIFO behavior. A queue would not be appropriate because the most recent operation needs to be undone first. A stack would be better because the last operation added is the first one removed.
## Scenario D: Breadth-First Search
### This scenario follows FIFO behavior. A queue is appropriate because vertices discovered earlier need to be processed before vertices discovered later. This allows BFS to explore the graph level by level.
## Scenario E: Function Calls
### This scenario follows LIFO behavior. A queue would not be appropriate because the most recently called unfinished function must complete before the function that called it resumes. A stack is appropriate because the most recently called function is handled first.

# Part 14: Queue and Breadth-First Search
## Step Vertex Processed Vertices Added Queue After Step
### Start — A [A]
### 1 A B, C [B, C]
### 2 B D, E [C, D, E]
### 3 C F [D, E, F]
### 4 D — [E, F]
### 5 E — [F]
### 6 F — []
### Analysis:
### 1. The vertices are visited in the order A, B, C, D, E, F.
### 2. The queue starts with A. After processing A, B and C are added. B is processed next because it was added first. D and E are then added behind C. C is processed next and adds F behind D and E. The remaining vertices are then processed in FIFO order.
### 3. FIFO behavior causes vertices to be explored level by level because vertices discovered at an earlier level are placed in the queue before vertices at later levels. This means vertices closer to the starting vertex are processed first.
### Complexity
### - BFS has O(V + E) time complexity when using adjacency lists. Each vertex is processed at most once, giving O(V), and each edge is examined while processing the adjacency lists, giving O(E).
### - The worst-case auxiliary space for the queue is O(V). In the worst case, the queue can contain a large portion of the graph's vertices at once, but it cannot contain more than V vertices.
