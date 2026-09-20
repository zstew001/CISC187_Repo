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
##
