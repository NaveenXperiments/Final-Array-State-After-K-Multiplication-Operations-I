# README.md

## Problem Description
The task is to modify an array `nums` after performing `k` operations. In each operation, the smallest element in the array is multiplied by a given `multiplier`, and the result is updated in the array. The process ensures that the smallest element is dynamically identified after every operation.

## Solution Explanation
This solution uses a **priority queue (min-heap)** to efficiently track and update the smallest element in the array. By storing the indices of elements in the priority queue, we can dynamically adjust priorities after each update, ensuring optimal performance even for larger arrays.

### Why Use a Priority Queue?
The **priority queue** (implemented as a min-heap in Java) is ideal for this problem because:
1. **Efficient Smallest Element Retrieval**:
   - The priority queue allows constant-time access to the smallest element and logarithmic-time updates, making it well-suited for dynamic data.
2. **Customizable Priority**:
   - The custom comparator ensures that elements are prioritized based on their values in `nums`. In case of ties (equal values), the element with the smaller index is given priority.
3. **Dynamic Adjustments**:
   - After every operation, the updated element is reinserted into the priority queue, automatically maintaining the correct order.

### Methodology
The solution is implemented as follows:

1. **Priority Queue Initialization**:
   - A priority queue is created with a custom comparator:
     ```java
     (i, j) -> nums[i] != nums[j] ? Integer.compare(nums[i], nums[j]) : Integer.compare(i, j)
     ```
     This ensures elements are sorted first by value in ascending order and then by index in case of ties.

2. **Adding Indices to the Queue**:
   - All indices of the `nums` array are added to the priority queue.

3. **Performing `k` Operations**:
   - For each operation:
     - The index of the smallest element is retrieved using `poll()`.
     - The corresponding value in `nums` is multiplied by the `multiplier`.
     - The updated index is reinserted into the priority queue to reflect the new value.

4. **Returning the Result**:
   - After `k` operations, the updated `nums` array is returned.

### Code Implementation
```java
class Solution {
    public int[] getFinalState(int[] nums, int k, int multiplier) {
        // Initialize a priority queue to store indices based on the custom comparator
        PriorityQueue<Integer> pq = new PriorityQueue<>((i, j) -> 
            nums[i] != nums[j] ? Integer.compare(nums[i], nums[j]) : Integer.compare(i, j));

        // Add all indices to the priority queue
        for (int i = 0; i < nums.length; i++) {
            pq.offer(i);
        }

        // Perform k operations
        while (k-- > 0) {
            int i = pq.poll();        // Get index of smallest element
            nums[i] *= multiplier;   // Update the value at that index
            pq.offer(i);             // Reinsert the index into the priority queue
        }

        return nums;
    }
}
```

### Complexity Analysis
#### Time Complexity
1. **Priority Queue Initialization**:
   - Adding `n` elements to the queue takes \(O(n \log n)\).
2. **`k` Operations**:
   - Each operation involves:
     - `poll()`: \(O(\log n)\).
     - `offer()`: \(O(\log n)\).
   - Total for `k` operations: \(O(k \log n)\).
3. **Overall Complexity**: \(O(n \log n + k \log n)\).

#### Space Complexity
- The priority queue stores up to `n` indices, resulting in \(O(n)\) space usage.

### Example Walkthrough
#### Input:
```java
nums = [4, 3, 2];
k = 2;
multiplier = 2;
```

#### Steps:
1. **Initialization**:
   - Priority queue contains indices `[0, 1, 2]` sorted based on `nums`: `[2, 1, 0]`.

2. **First Operation**:
   - Poll index `2` (smallest value `2`).
   - Update `nums[2] = 2 * 2 = 4`.
   - Reoffer index `2`.
   - Updated queue: `[1, 0, 2]`.
   - Updated array: `[4, 3, 4]`.

3. **Second Operation**:
   - Poll index `1` (smallest value `3`).
   - Update `nums[1] = 3 * 2 = 6`.
   - Reoffer index `1`.
   - Updated queue: `[0, 2, 1]`.
   - Updated array: `[4, 6, 4]`.

#### Output:
```java
nums = [4, 6, 4];
```

### Advantages of the Priority Queue Approach
1. **Efficiency**:
   - Avoids repeatedly scanning the array for the smallest element.
2. **Dynamic Handling**:
   - Handles changes in the array without re-sorting.
3. **Scalability**:
   - Works well even for larger arrays and values of `k`.

---

With the priority queue, this problem is solved efficiently and cleanly, leveraging the dynamic nature of heaps to maintain optimal performance throughout the operations.
