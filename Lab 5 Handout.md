# CS2040DE Data Structures and Algorithms

# Lab 5: Individual Assignment, Heap Data Structure and Priority Queue

<div align="center">
<img src="./images/nus_logo.jpg" alt="NUS Logo" style="zoom:30%;" />
</div>





<div align="center">
<table style="border-collapse: collapse; margin: 0 auto;">
<tr><td align="center" style="font-size: 24px;"><strong>National University of Singapore</strong></td></tr>
<tr><td align="center" style="font-size: 24px;"><em>Academic Year 2024/25 Semester 2</em></td></tr>
<tr><td align="center" style="font-size: 16px; padding-top: 10px;">Hitesh (B03) and Zikun (B01 & B04)</td></tr>
<tr><td align="center" style="font-size: 14px;"><em>Materials adapted from: Xiaoyang, Hitesh, Yurui</em></td></tr>
</table>
</div>


## Learning Objectives

By the end of this lab, students should be able to:

- [1] Know exactly about what individual assignment is asking for
- [2] Recap about binary heaps
- [3] Recap about priority queues and how to use them in Java
- [4] Do some questions
- [5] (Optional) Get other questions from previous labs answered
- [6] (Optional) Find more practice problems to do if they wish

## Individual Assignment Briefing

Your lab tutor will brief you on the individual assignment question, skeleton code, and a platform to test your code.



## Binary Heap

### Full Binary Tree

A **full binary tree** is a binary tree where every node has either 0 or 2 children. **No node has just one child.** It is a binary tree with a depth of $k$ and exactly $2^k-1$ nodes. Every level of a full binary tree contains the maximum possible number of nodes. 

### Complete Binary Tree

Every level in the binary tree, except possibly the last/lowest level, is **completely filled**, and all vertices in the last level are **as far left as possible**.

From the definition of full binary tree and complete binary tree, it can be seen that the full binary tree is a special form of complete binary tree. That is, if a binary tree is a full binary tree, it must be a complete binary tree.

### Binary Heap Data Structure

A **binary heap** is a special type of complete binary tree that follows a specific order property:

- **Max-Heap**: The value of each node is greater than or equal to the values of its children.
- **Min-Heap**: The value of each node is less than or equal to the values of its children.

In both cases, the root node represents the largest (in max-heap) or smallest (in min-heap) value.

**Question:** Can we say that the root of a Max-heap is the largest element in the heap? Can we say that all nodes in the Max-heap are sorted?  Can we say that all nodes in the previous (parent) level are larger than all nodes in the next (child) level?

#### 1. **Using an Array to Build a Binary Heap**

In a binary heap, the parent-child relationship in a tree can be represented using a simple array. For any element at index `i`:

- **Parent index** = $(i-1) / 2$
- **Left child index** = $2i + 1$
- **Right child index** = $2i + 2$

For example, consider the following max-heap represented as an array:

```
        50
      /    \
    30      20
   /  \    /
 15   10  8
```

The array representation of this heap would be: `[50, 30, 20, 15, 10, 8]`.

#### 2. (Optional) Binary **Heap Operations**

Most of the heap operations like `insert()`, `delete()`, `extract_max()` (or `extract_min()`), and `build_heap()` are fundamentally based on **heapify** operations. In both **max-heaps** and **min-heaps**, the heap property must be maintained, and this is achieved by **heapify**. Thus, implementing `heapify()` is crucial before implementing other operations.

**Heapify down** is used after an element has been removed or a replacement has been made (for example, when deleting the root), to move the out-of-place element to its correct position. In a max-heap, we need to ensure that the value of a parent node is always greater than or equal to the values of its children. Here’s the code for **heapify down**:

```java
// Heapify down for max-heap
private void heapifyDown(int index) {
    int largest = index;  // Assume current index is the largest
    int left = 2 * index + 1;  // Left child index
    int right = 2 * index + 2; // Right child index

    // Compare the parent with the left child
    if (left < size && heap[left] > heap[largest]) {
        largest = left;
    }

    // Compare the largest value so far with the right child
    if (right < size && heap[right] > heap[largest]) {
        largest = right;
    }

    // If the largest value is not the current index, swap and continue heapifying
    if (largest != index) {
        int temp = heap[index];
        heap[index] = heap[largest];
        heap[largest] = temp;

        // Recursively heapify the affected subtree
        heapifyDown(largest);
    }
}
```

For a **min-heap**, the process is the same, except you swap if the parent is **greater** than the child, and you swap with the smaller child.

**Heapify up** (sometimes called "bubble up") is used when inserting a new element. The new element is initially placed at the end of the array, and it's moved up the tree until the heap property is restored. You need to check if the inserted element is greater (for a max-heap) or smaller (for a min-heap) than its parent. 

```java
// Heapify up for max-heap
private void heapifyUp(int index) {
    // Check the parent node
    while (index > 0 && heap[(index - 1) / 2] < heap[index]) {
        // Swap the current node with its parent
        int temp = heap[index];
        heap[index] = heap[(index - 1) / 2];
        heap[(index - 1) / 2] = temp;

        // Move up the tree
        index = (index - 1) / 2;
    }
}
```

Similarly, for the min-heap, you need to check $heap[(index - 1) / 2] > heap[index]$.



##### **Building the Binary Heap**

To **build a binary heap** from an unsorted array, we need to apply the **heapify down** operation from the bottom-most non-leaf node to the root. This ensures that all subtrees are valid heaps. The time complexity of building a heap is **O(n)**.

```java
// Build a max-heap from an unsorted array
public void buildHeap() {
    for (int i = size / 2 - 1; i >= 0; i--) {
        heapifyDown(i);
    }
}
```



##### **Insert Operation**

When you insert a new element, you first add it to the end of the array and then apply **heapify up** to move the element to its correct position.

```java
// Insert a new element into the max-heap
public void insert(int value) {
    heap[size] = value;  // Place the new element at the end
    int current = size;  // Start from the inserted position
    size++;              // Increase the size of the heap

    heapifyUp(current);  // Heapify up to restore the heap property
}
```



##### **Extract Max/Min (Delete the Root)**

The **extract_max()** (or **extract_min()** for min-heap) operation is essentially removing the root element (which is the maximum or minimum). After removing the root, you replace it with the last element in the heap and then apply **heapify down** to restore the heap property.

```java
// Extract the max element from the max-heap
public int extractMax() {
    if (size == 0) throw new IllegalStateException("Heap is empty");

    int max = heap[0];  // Get the root element
    heap[0] = heap[size - 1];  // Replace it with the last element
    size--;  // Reduce the size of the heap

    heapifyDown(0);  // Heapify down to restore the heap property

    return max;  // Return the removed element
}
```

For a **min-heap**, you would use the same logic but instead, extract the minimum and heapify down using the `heapifyDownMin()` method.



## Priority Queue

A priority queue is like a queue in that it stores elements and allows you to “enqueue” and “dequeue.” The key difference is in how “dequeue” works:

- Normal queue: dequeue removes the earliest inserted element (FIFO).
- Priority queue: dequeue removes the element with the highest (or lowest) priority. In code, we often say highest priority means largest key for a max-priority-queue, or smallest key for a min-priority-queue.



In a priority queue:

- `enqueue(x)`: Insert a new element `x` into the priority queue.

- `y = dequeue()`: Remove the “highest priority” element `y` (or by default, the **smallest** element in Java’s `PriorityQueue` since it’s actually a min-heap).



You can create a **priority queue** of integers in Java simply by:

```java
import java.util.PriorityQueue;

PriorityQueue<Integer> pq = new PriorityQueue<>();
```



By **default**, Java’s `PriorityQueue` is a **min-heap**: calling `pq.poll()` returns the smallest element. If you need a **max-heap** behavior, you can provide a **comparator** such as:

```java
PriorityQueue<Integer> maxPQ = new PriorityQueue<>(Collections.reverseOrder());
```

Now, `maxPQ.poll()` returns the **largest** element first.



the `PriorityQueue` class is backed by a **binary heap** (an array-based heap). The elements are stored in an array, and the usual heap operations (`sift up`, `sift down`) are performed internally whenever you add or remove elements, guaranteeing $O(logn)$ time for `offer` (enqueue) and `poll` (dequeue). The entire data structure always satisfies the **heap property**—specifically, a *min-heap* by default.



# Optional Questions From Previous Semester

Q1: [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

Q2: [Array Smoothening](https://open.kattis.com/contests/if9tod/problems/arraysmoothening)

Q3: [Jane Eyre](https://open.kattis.com/contests/mzvgmb/problems/janeeyre)

Q4: [Canvas Painting](https://open.kattis.com/contests/pmed3i/problems/canvas)

Q5: [Knigs of the Forest](https://open.kattis.com/contests/a7ofuc/problems/knigsoftheforest)

Q6: [I Can Guess the Data Structure!](https://open.kattis.com/contests/iajvhr/problems/guessthedatastructure)

Q7: [Annoyed Coworkers](https://open.kattis.com/contests/fdmyst/problems/annoyedcoworkers)
