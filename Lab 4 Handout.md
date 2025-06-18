# CS2040DE Data Structures and Algorithms

# Lab 4: Set (and Recap of Lab 3 Content)

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

- [1] Recap about trees
- [2] Recap about binary trees, focusing on their unique two-child structure and fundamental properties
- [3] Study two implementation methods for binary trees - using arrays (with index relationships) and linked lists (with node references)
- [4] Master different binary tree traversal techniques, including depth-first methods (preorder, inorder, postorder) and breadth-first traversal, with Java implementation practice
- [5] Understand binary search trees (BST) and their connection to sets, followed by practical Java set programming exercises
- [6] (Optional) Get other questions from previous labs answered
- [7] (Optional) Find more practice problems to do if they wish



## Quick Recap of Trees

![Screen Shot 2025-02-15 at 10.59.18 PM](./images/Screen%20Shot%202025-02-15%20at%2010.59.18%20PM.png)

![Screen Shot 2025-02-15 at 10.59.28 PM](./images/Screen%20Shot%202025-02-15%20at%2010.59.28%20PM.png)

![Screen Shot 2025-02-15 at 10.59.39 PM](./images/Screen%20Shot%202025-02-15%20at%2010.59.39%20PM.png)

## Quick Recap of Binary Trees

![Screen Shot 2025-02-15 at 10.58.52 PM](./images/Screen%20Shot%202025-02-15%20at%2010.58.52%20PM.png)

![Screen Shot 2025-02-15 at 10.59.51 PM](./images/Screen%20Shot%202025-02-15%20at%2010.59.51%20PM.png)



## Quick Recap of Binary Search Tree

![Screen Shot 2025-02-15 at 11.00.24 PM](./images/Screen%20Shot%202025-02-15%20at%2011.00.24%20PM.png)

![Screen Shot 2025-02-15 at 11.00.38 PM](./images/Screen%20Shot%202025-02-15%20at%2011.00.38%20PM.png)





## How to Implement a Binary Tree: Array-Based and Pointer-Based 

**A binary tree can be stored using either linked storage or sequential storage.**

In linked storage, we use **pointers**, while in sequential storage, we use **arrays**. As the names suggest, sequential storage means elements are distributed continuously in memory, while linked storage connects nodes distributed across different memory addresses through pointers.



**Linked Storage:**

![Screen Shot 2025-02-15 at 11.15.51 PM](./images/Screen%20Shot%202025-02-15%20at%2011.15.51%20PM.png)



**Sequential Storage:**

Linked storage is a very familiar method to everyone, so let's take a look at how sequential storage works?

In fact, it's using arrays to store binary trees, the sequential storage method is shown in the figure:

![Screen Shot 2025-02-15 at 11.03.20 PM](./images/Screen%20Shot%202025-02-15%20at%2011.03.20%20PM.png)



![Screen Shot 2025-02-15 at 11.03.37 PM](./images/Screen%20Shot%202025-02-15%20at%2011.03.37%20PM.png)



How do we traverse a binary tree stored in an array? **If a parent node's array index is i, then its left child's index is i \* 2 + 1, and its right child's index is i \* 2 + 2.** However, representing a binary tree using linked structure is more helpful for our understanding, so we generally use linked storage for binary trees. **So everyone should understand that arrays can still be used to represent binary trees.**



## Binary Tree Traversal

Some students who have solved many binary tree problems may be familiar with pre-order, in-order, post-order, and level-order traversals, but might lack a comprehensive framework to understand them.

### Two Main Categories of Tree Traversal

1. **Depth-First Search (DFS)**: Goes deep into the tree first, then backtracks when reaching leaf nodes.
2. **Breadth-First Search (BFS)**: Traverses the tree level by level.

*Note: These are the two fundamental traversal methods in graph theory, which we'll explore more when discussing graphs.*

### Detailed Traversal Methods

#### Depth-First Search (DFS)

- Pre-order Traversal (**recursive** and iterative)
- In-order Traversal (**recursive** and iterative)
- Post-order Traversal (**recursive** and iterative)

#### Breadth-First Search (BFS)

- Level-order Traversal (recursive and **iterative**)

### Understanding DFS Order

Many students get confused about pre-order, in-order, and post-order traversals. Here's a helpful tip: **The "order" refers to when we visit the root/middle node relative to its children.**

For any node:

- Pre-order: Root → Left → Right
- In-order: Left → Root → Right
- Post-order: Left → Right → Root

### Implementation Methods

#### DFS Implementation

- Typically implemented using recursion for pre-order, in-order, and post-order traversals
- Can also be implemented using a stack (remember: stack is essentially an iterative version of recursion)

#### BFS Implementation

- Typically implemented using a queue
- Queue's First-In-First-Out (FIFO) property naturally supports level-by-level traversal

*Note: When dealing with binary tree problems, recursive implementation is often more convenient for DFS traversals.*

(Optional) **Tips for Recursion** ("Trilogy of Recursion"):

**1. Determining Function Parameters and Return Type**

When designing a recursive function, we need to identify:

- What information needs to be passed between recursive calls
- What the function should return at each step

For preorder traversal, we need:

- Parameters: We need the root node to traverse, and a List to store our values

- Return Type: Since we're collecting values in our passed List, we don't need to return anything (void)

- ```java
  private void preorderHelper(TreeNode node, List<Integer> result)
  ```

**2. Establishing the Base Case (Termination Condition)** The base case prevents stack overflow by stopping recursion. Without it, the program would try to process nodes indefinitely, causing the system's call stack to overflow.

For preorder traversal, our base case is simple:

```java
if (node == null) {
    return;
}
```

**3. Defining Single-Level Recursive Logic** This is where we define what happens at each recursive step. For preorder traversal, we follow the "Root-Left-Right" pattern:

```java
// Process current node (Root)
result.add(node.val);

// Process left subtree
preorderHelper(node.left, result);

// Process right subtree
preorderHelper(node.right, result);
```

## Set

A **Set** is a collection that:

- Contains no duplicate elements.
- Has no particular order unless explicitly defined.
- Offers efficient membership checking, insertion, and deletion.

Java provides several implementations of **Set**, including:

1. **HashSet**: Based on a **hash table**, which provides constant-time performance for basic operations (O(1)) such as adding, removing, and checking for elements. However, the order of elements is undefined.
2. **TreeSet**: Based on a **Red-Black tree**, a type of self-balancing binary search tree (BST). This implementation ensures that the elements are stored in **sorted order** and provides logarithmic-time performance for most operations (O(log n)).

For this tutorial, we'll focus more on **TreeSet**, as it operates similarly to a BST.

#### Key Features of Set:

- **Uniqueness**: A **Set** does not allow duplicate elements.
- **Efficient Operations**: Most sets provide efficient operations for insertion, deletion, and lookup.
- **Sorted or Unsorted**: Depending on the implementation (HashSet or TreeSet), the Set can either maintain the elements in a sorted manner or have no guaranteed order.

#### **TreeSet**: Set Based on a BST

The **TreeSet** in Java is backed by a **Red-Black tree**. Red-Black trees are a type of self-balancing binary search tree that maintains the following properties:

- **Binary Search Tree (BST)** property: Left child nodes are smaller than the parent node, and right child nodes are larger than the parent node.
- **Self-balancing**: The height of the tree is kept in check, ensuring that no path in the tree is more than twice as long as any other.

This ensures that operations like insertion, deletion, and lookup are done in $O(\log n)$ time, where $n$ is the number of elements in the set.

#### **Common Operations in TreeSet**:

##### **Adding an Element**:

```java
TreeSet<Integer> treeSet = new TreeSet<>();
treeSet.add(5);
treeSet.add(10);
treeSet.add(1);
```

The element is inserted in **sorted order**. For example, after the above code runs, the `treeSet` will contain [1, 5, 10].

##### **Removing an Element**:

```java
treeSet.remove(5);
```

The element `5` will be removed, and the tree will still maintain the sorted order. After removal, the tree will have [1, 10].

##### **Checking if an Element Exists**:

```java
boolean containsTen = treeSet.contains(10);  // returns true
```

This operation checks whether the given element is present in the set.

##### **Iterating Over the Set**:

```java
for (Integer num : treeSet) {
    System.out.println(num);
}
```

Since `TreeSet` maintains the elements in sorted order, iterating over the set will give you the elements in ascending order.

##### **First and Last Elements**:

```java
int first = treeSet.first();  // returns 1
int last = treeSet.last();  // returns 10
```

For more operations, you can refer to the Appendix.



Internally, the **TreeSet** in Java uses a **Red-Black tree**, which is a type of self-balancing binary search tree (BST). In simpler terms, it is a specialized BST that ensures:

1. **BST Properties**:
   - For each node, all values in the left subtree are smaller than the node's value, and all values in the right subtree are larger.
2. **Self-Balancing**:
   - The tree balances itself during insertions and deletions. This ensures that the height of the tree remains $O(\log n)$, and operations like insertion, deletion, and lookup are all logarithmic in time.
3. **No Duplicates**:
   - The tree does not allow duplicate values. If you try to add a value that already exists in the tree, it simply won’t be added again.

If we were to implement a Set using a plain **Binary Search Tree** (BST), the underlying structure would be similar to how **TreeSet** is implemented but without the balancing.



# Optional Questions From Previous Semester

Q1: [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

Q2: [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

Q3: [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)

Q4: [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)

Q5: [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/description/)

Q6: [Binary search tree](https://open.kattis.com/contests/pm58vz/problems/bst) 

Q7: [Compound Words](https://open.kattis.com/problems/compoundwords)

Q8: [Candy Division](https://open.kattis.com/contests/wcgxqt/problems/candydivision)

Q9: [Palindromic Password](https://open.kattis.com/contests/iqgt3k/problems/palindromicpassword)



## Appendix

##### 1. **Basic Operations**

- **`add(E e)`**: Inserts the specified element into the `TreeSet` if it is not already present.
- **`remove(Object o)`**: Removes the specified element from the `TreeSet`.
- **`contains(Object o)`**: Returns `true` if the `TreeSet` contains the specified element.
- **`size()`**: Returns the number of elements in the `TreeSet`.

##### 2. **Accessing the Smallest and Largest Elements**

- **`first()`**: Returns the first (lowest) element currently in the `TreeSet`.
- **`last()`**: Returns the last (highest) element currently in the `TreeSet`.

##### 3. **Navigational Methods**

- **`lower(E e)`**: Returns the greatest element in this set strictly less than the given element, or `null` if there is no such element.
- **`higher(E e)`**: Returns the least element in this set strictly greater than the given element, or `null` if there is no such element.
- **`floor(E e)`**: Returns the greatest element in this set less than or equal to the given element, or `null` if there is no such element.
- **`ceiling(E e)`**: Returns the least element in this set greater than or equal to the given element, or `null` if there is no such element.

##### 4. **Subsets and Views**

- **`subSet(E fromElement, E toElement)`**: Returns a view of the portion of this set whose elements range from `fromElement`, inclusive, to `toElement`, exclusive.
- **`headSet(E toElement)`**: Returns a view of the portion of this set whose elements are strictly less than `toElement`.
- **`tailSet(E fromElement)`**: Returns a view of the portion of this set whose elements are greater than or equal to `fromElement`.

##### 5. **Poll Methods**

These methods retrieve and remove elements from the `TreeSet`:

- **`pollFirst()`**: Retrieves and removes the first (lowest) element, or returns `null` if the set is empty.
- **`pollLast()`**: Retrieves and removes the last (highest) element, or returns `null` if the set is empty.

##### 6. **Iterating through the TreeSet**

- **`iterator()`**: Returns an iterator over the elements in ascending order.
- **`descendingIterator()`**: Returns an iterator over the elements in descending order.

##### 7. **Additional Set Operations**

- **`isEmpty()`**: Returns `true` if the set contains no elements.
- **`clear()`**: Removes all elements from the set.
- **`comparator()`**: Returns the comparator used to order the elements in this set, or `null` if it uses the natural ordering of the elements.
