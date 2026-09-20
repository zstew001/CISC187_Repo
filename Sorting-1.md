# Week 3/4: Sorting Pt. 1

# Question 1: Linear Complexity
### An algorithm requires approximately 4N + 16 steps for an input of size N. Use *Big-O notation* to describe the time-complexity of this algorithm.
## The algorithm has a time-complexity of O(N). Big-O notation is used to describe the growth rate of the algorithm as the input size for N increases. The constant multiplier, 4, and constant term, 16, don't change the overall linear growth rate of the algorithm. While the 4 and + 16 will increase the total number of steps the algorithm takes, compared to just N, the algorithm is still classified as O(N) because it grows at a linear rate.

# Question 2: Quadratic Complexity
### An algorithm requires approximately 2N² steps for an input size N. Use *Big-O notation* to describe the time-complexity of this algorithm.
## The algorithm has a time-complexity of O(N²). The growth rate of the algorithm is quadratic in this case. As N increases, the number of steps the algorithm takes increases exponentially. For example, if N = 2, the algorithm requires 2(2 x 2) (8) steps to complete. If N = 100, the algorithm requires 2(100 x 100) (20,000) steps to complete. Lets compare this to the previous algorithm, which takes 4N + 16 steps. With an input size N = 100, the algorithm would take 4(100) + 16 (416) steps. As you can see, as N increases, a linear time-complexity algorithm is significantly faster than an algorithm with quadratic time-complexity.

# Question 3: Analyzing Multiple Sequential Loops
```Ruby
def double_then_sum(array)
    doubled_array = []

    array.each do |number|
        doubled_array << number * 2
    end

    sum = 0

    doubled_array.each do |number|
        sum += number
    end

    return sum
end
```
### Analyze the time-complexity of this function using *Big-O notation*.
## The first loop doubles each element in the array. For N elements the first loop must execute N times. Similarly, the second loop adds each array element to the sum and for N elements it must execute N times. So the algorithm requires roughly N + N (2N) steps for an input size of N. Since we can ignore the constant multiplier, the algorithm falls under O(N) time-complexity. Having two sequential loops doesn't change the overall time-complexity of the function because the two loops act in a way that is additive, rather than multiplicative. If the algorithms were nested rather than sequential, the time complexity would change to N x N (N²). This is due to the fact that in a set of nested loops, each operation would also trigger another set of N operations.

# Question 4: Multiple Constant-Time Operations
```Ruby
def multiple_cases(array)
    array.each do |string|
        puts string.upcase
        puts string.downcase
        puts string.capitalize
    end
end
```
### Analyze the time-complexity of this function using *Big-O notation*.
## The loop executes N times for N strings. For each loop execution, the function must perform 3 string operations. So for an array of N elements, the time-complexity of the function is 3N. Even though there are a set of constant operations inside the loop, the algorithm still has a linear time-complexity of O(N). This is because the operations within the loop are constant, regardless of the value of N.  

# Question 5: Analyzing Nested Iteration
```Ruby
def every_other(array)
    array.each_with_index do |number, index|
        if index.even?
            array.each do |other_number|
                puts number + other_number
            end
        end
    end
end
```
### The function iterates through an array of numbers. For elements at even indices, it iterates through the entire array and prints the sum of the selected number and every other number.
### Analyze the efficiency of this function using Big-O notation.
## In this function the outer loop executes N times for N elements. The condition: index.even will be true roughly 50% of the time so the inner loop will run approximately N/2 times. Since the inner loop is dependent on the outer loop, the inner loop operates N x N/2 times, which simplifies to N²/2. Following what was shown in the previous questions, the final Big-O notation doesn't take into account the constant multiplier so the final Big-O time-complexity is roughly O(N²). Even though the inner loop of the function only processes every other element, it is still dependent on the amount of operations the outer loop performs. In this way, the two loops interact multiplicatively. Linear N x Linear N/2 still produces a quadratic time-complexity.

# Analysis and Reflection:
## Based on the previous problems, we can see that Big-O notation is used to describe growth-rate of an algorithm as the input size increases. Even though constant multipliers may increase or reduce the total number of operation required, they don't change the growth pattern. Additionally, O(N) (Linear growth) and O(N²) (Quadratic growth) diverge significantly as input size increases. An algorithm with linear growth requires an additive amount of work as N increases. On the other hand, an algorithm with quadratic growth requires a multiplicative amount of work as N increases. For O(N), if N doubles, the number of operations also doubles. For O(N²), if N doubles, the number of operations required quadruples. This is important to consider when deciding to use sequential or nested loops. A pair of sequential loops adds the operations done by each loop. On the other hand, a nested loop multiplies the number of operations because the inner loop is dependent on the operations of the outer loop. Understanding these interactions is incredibly important as the size of a dataset grows because as N becomes larger, the number of operations required by the algorithm can scale out of control very quickly. For example, an algorithm with O(N) time complexity will perform ~100,000 operations for a dataset of 100,000 elements. An algorithm with O(N²) would perform ~10 Billion operations. It's clear that choosing an algorithm with appropriate time complexity for the size of a dataset massively impacts time taken and resources used.
