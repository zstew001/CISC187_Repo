# Space Constraints

## Part 1 Analysis: Time Complexity vs. Space Complexity

| Statement | Time, Space, or Both? | Reasoning |
| :--- | :--- | :--- |
| The number of operations grows linearly with N. | Time | The algorithm performs more operations as N increases. |
| The algorithm creates a second vector containing N elements. | Space | The new vector requires additional memory that grows with N. |
| The algorithm performs nested loops but creates no additional data structure that grows with N. | Both | Nested loops affect running time, but the additional memory stays constant. |
| A recursive function generates N simultaneously active stack frames. | Space | Each active call uses stack memory, so the amount of memory grows with N. |

## Part 2 Analysis: Additional Storage vs. In-Place Modification

| Question | Version 1 | Version 2 |
| :--- | :--- | :--- |
| Creates an additional container that grows with N? | Yes | No |
| Auxiliary-space complexity | O(N) | O(1) |
| Modifies the original input? | No | Yes |

### 1. Version 2 is considered in-place because it changes the strings in the original vector instead of creating another vector to store the results.
### 2. O(1) auxiliary space doesn't mean that no memory is being used. It means the additional memory doesn't increase as N grows.
### 3. Modifying the input in place could be a problem if the original values need to be used again later, because the original strings would no longer be unchanged.

## Part 3 Analysis: Duplicate Detection — Version 1

### 1. N represents the number of values stored in the input vector.
### 2. The time complexity is O(N²) because each value may be compared with every other value through the nested loops.
### 3. No additional memory grows with N. The function only uses the loop variables and comparisons.
### 4. The auxiliary-space complexity is O(1).
### 5. Nested loops can increase the number of operations without increasing memory usage. The loops repeat comparisons, but they don't create any additional storage that grows with N.

## Part 4 Analysis: Duplicate Detection — Version 2

### 1. This approach can run in O(N) average time because it checks each value once and hash-table lookups are O(1) on average.
### 2. In the worst case for memory, `seen` could store N distinct values if there aren't any duplicates.
### 3. The auxiliary-space complexity is O(N) because the hash table can grow with the number of input values.
### 4. The additional memory used by `seen` is exchanged for faster searching, instead of comparing every value against the others.

## Part 5 Analysis: Duplicate Detection — Version 3

### 1. Sorting puts equal values next to each other, so duplicates can be found by comparing neighboring elements.
### 2. The final scan is O(N) because it only moves through the vector once.
### 3. The overall time is O(N log N) because sorting takes longer than the O(N) scan.
### 4. Under the lecture's model, the auxiliary-space complexity is O(log N) due to the memory used by the sorting process.
### 5. The original ordering of `values` is changed because the vector is sorted before checking for duplicates.

## Part 6 Analysis: Compare the Three Algorithms

| Approach | Time Complexity | Auxiliary Space | Modifies Input? | Reasoning |
| :--- | :--- | :--- | :--- | :--- |
| Nested loops | O(N²) | O(1) | No | Compares values repeatedly but doesn't create additional storage. |
| Hash table | O(N) average | O(N) | No | Checks each value once on average, but `seen` may store N values. |
| Sort + adjacent scan | O(N log N) | O(log N) | Yes | Sorting takes O(N log N) time and uses O(log N) extra space under the lecture's model. |

## Part 7 Analysis: Time-Space Trade-off Decisions

### Scenario A - Memory-Constrained Device:
I would use nested loops because they only require O(1) additional space. The O(N²) time is slower, but memory is the main limitation in this scenario.

### Scenario B - Speed-Critical Application:
I would use the hash table because it runs in O(N) average time. It requires O(N) additional space, but the server has enough memory and speed is more important.

### Scenario C - Balanced Constraints:
I would use sorting and checking adjacent values because it avoids O(N²) time while only requiring O(log N) auxiliary space under the lecture's model. The trade-off is that the input order changes.

### Scenario D - Input Order Must Be Preserved:
I would use the hash table if there's enough memory because it doesn't change the original vector and still runs in O(N) average time. If memory is extremely limited, nested loops would preserve the order with O(1) additional space, but would take O(N²) time.

## Part 8 Analysis: The Hidden Space Cost of Recursion

| Question | Your Analysis |
| :--- | :--- |
| Active values of n before unwinding begins | `5, 4, 3, 2, 1, 0, -1` |
| Approximate maximum number of active stack frames as a function of N | `N + 2`, or O(N), including the stopping call |
| Time complexity | O(N) |
| Auxiliary-space complexity | O(N) |

### Analysis:
The function doesn't have O(1) auxiliary space because each recursive call adds another stack frame. Even though it doesn't create a vector or array, the number of unfinished calls increases with N.

## Part 9 Analysis: Recursion vs. Iteration

| Property | Recursive Version | Iterative Version |
| :--- | :--- | :--- |
| Time complexity | O(N) | O(N) |
| Auxiliary-space complexity | O(N) | O(1) |
| Uses repeated stack frames? | Yes | No |

### Analysis:
Both versions take O(N) time because they count down through the same values. However, recursion keeps adding stack frames until it reaches a stopping condition, while the loop uses the same variables without making additional function calls.

## Part 10 Analysis: Peak Call-Stack Analysis

| Situation | Maximum Stack Depth | Auxiliary Space | Reasoning |
| :--- | :--- | :--- | :--- |
| Recursive countdown from N to 0 | O(N) | O(N) | Each call decreases N by 1, leaving ~N calls active before returning. |
| Recursive algorithm with depth proportional to log N | O(log N) | O(log N) | There are only about log N active calls at the deepest point. |

### Analysis:
This is similar to the Quicksort example, where balanced recursive calls can use O(log N) stack space. The important thing is how many calls are active at once, not the total number of calls made.

## Part 11 Analysis: Final Algorithm Selection

### 1. I would choose Algorithm A if memory is extremely limited because it only uses O(1) additional space. The downside is that O(N²) time can become very slow as N increases.

### 2. I would choose Algorithm B if execution speed is the highest priority because it runs in O(N) time. It uses O(N) additional memory, but that's the trade-off for faster execution.

### 3. I would choose Algorithm C if both memory and time are constrained. O(N log N) is faster than Algorithm A, while O(log N) auxiliary space uses less memory than Algorithm B.

### 4. For a very large N, I would choose Algorithm B if enough memory is available because O(N) time avoids the quadratic time-complexity. Algorithm C would also be reasonable if O(N) additional space is too much, since it still avoids O(N²) time-complexity.

## Analysis and Reflection:

Auxiliary space is the additional memory an algorithm uses beyond the original input. O(1) doesn't mean no memory is used, only that the amount of additional memory stays the same as N grows. If an algorithm creates another vector with N elements, it needs O(N) auxiliary space because the storage increases with the input size. An in-place algorithm can use less memory because it changes the original values instead of making another copy.

The fastest algorithm isn't always the best choice because it may require more memory than a system can handle. At the same time, using the least amount of memory isn't always better if the algorithm takes too long to finish. This was the difference between the nested loops, hash table, and sorting approaches. Each one has different time and space requirements, so the better choice depends on what the system needs.

Recursion can also use additional memory even when there isn't an array or vector being created. Every recursive call adds a stack frame, and the maximum number of active calls determines how much stack space is needed. This is why the recursive countdown uses O(N) auxiliary space while the loop only uses O(1). When choosing an algorithm, both execution time and available memory need to be considered rather than just selecting whichever one has the lowest Big O time complexity.
