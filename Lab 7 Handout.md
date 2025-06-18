# CS2040DE Data Structures and Algorithms

# Lab 7: More about Graph Theory, DFS

<div align="center">
<img src="images/nus_logo.jpg" alt="NUS Logo" style="zoom:30%;" />
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

- [1] Learn about a very simple way to implement an edge list, adjacency list and adjacency matrix to store a graph in Java
- [2] Learn about how to implement DFS in Java and avoid common mistakes
- [3] Understand completely what are the similarities and differences between DFS and BFS
- [4] Do two questions relating to DFS in Java
- [5] (Optional) Get other questions from previous labs answered
- [6] (Optional) Find more practice problems to do if they wish

## More About Graph Representations (i.e., Edge List, Adjacency Matrix and Adjacency List)

### Graph Representation Comparison Table

|                                                 |                          Edge List                           |                       Adjacency Matrix                       |                        Adjacency List                        |
| :---------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|                   Definition                    | Stores all edges as a an array of array (i.e., $(u,v)$/$(u,v,w)$) | Stores a 2D matrix (size $V×V$) where each cell $(i,j)$ indicates an edge’s presence | Stores, for each vertex, a list of adjacent (connected) vertices |
|                  How to store                   |           array of array (i.e., pairs / triplets)            |                   2D array of size $V × V$                   |                    array of linked lists                     |
|                Space complexity                 |                            $O(E)$                            |                           $O(V^2)$                           | $O(V + E)$<br /><br />Because each vertex and its adjacency connections are recorded. |
| Memory usage for dense graphs (a lot of edges)* |             Less efficient than adjacency matrix             |                        Most efficient                        |             Less efficient than adjacency matrix             |
| Memory usage for sparse graphs (very few edges) |                          Efficient                           |                  Inefficient (wastes space)                  |                          Efficient                           |
|            Implementation complexity            |                            Simple                            |                            Simple                            |                           Moderate                           |

Consider a graph with 1,000 vertices:

- Adjacency matrix: Always 1,000,000 entries

- If the graph has 900,000 edges (very dense, 90% of possible edges):

  - Edge list: 1,800,000 entries (two vertices per edge)
  - Adjacency list: 1,000 + 1,800,000 entries (headings plus edges)

  

### Time Complexity for Common Operations

|                                  |         Edge List          |           Adjacency List           | Adjacency Matrix                   |
| :------------------------------: | :------------------------: | :--------------------------------: | ---------------------------------- |
| Check if two nodes are connected | O(E) - must scan all edges |   O(V) - must scan neighbor list   | O(1) - direct lookup               |
|     Find all adjacent nodes      | O(E) - must scan all edges |    O(V) - direct access to list    | O(V) - must scan entire row/column |
|      Iterate over all edges      |    O(E) - direct access    | O(V + E) - must traverse all lists | O(V²) - must check all cells       |



## Implementation in Java

![Screen Shot 2025-03-24 at 9.23.01 AM](images/Screen%20Shot%202025-03-24%20at%209.23.01%20AM.png)

```java
import java.util.LinkedList;

public class SimpleGraphRepresentations {
    public static void main(String[] args) {
        // 1. Edge List representation
        // Simple 8x2 array where each row represents an edge
        int[][] edgeList = new int[8][2];
        
        // Adding the edges from the graph
        edgeList[0] = new int[]{6, 7};
        edgeList[1] = new int[]{6, 8};
        edgeList[2] = new int[]{7, 8};
        edgeList[3] = new int[]{1, 2};
        edgeList[4] = new int[]{2, 5};
        edgeList[5] = new int[]{5, 4};
        edgeList[6] = new int[]{4, 3};
        edgeList[7] = new int[]{3, 1};
        
        // Print edge list
        System.out.println("Edge List:");
        for (int[] edge : edgeList) {
            System.out.println(edge[0] + " -> " + edge[1]);
        }
        System.out.println();
        
        // 2. Adjacency Matrix representation
        // 8x8 array where each cell [i][j] indicates if edge from i to j exists
        int[][] adjMatrix = new int[8][8]; // initialized with zeros
        
        // Add edges (setting 1 in cells where edges exist)
        adjMatrix[6-1][7-1] = 1; // 6 -> 7 (adjusting indices if needed)
        adjMatrix[6-1][8-1] = 1; // 6 -> 8
        adjMatrix[7-1][8-1] = 1; // 7 -> 8
        adjMatrix[1-1][2-1] = 1; // 1 -> 2
        adjMatrix[2-1][5-1] = 1; // 2 -> 5
        adjMatrix[5-1][4-1] = 1; // 5 -> 4
        adjMatrix[4-1][3-1] = 1; // 4 -> 3
        adjMatrix[3-1][1-1] = 1; // 3 -> 1
        
        // Print adjacency matrix
        System.out.println("Adjacency Matrix:");
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 8; j++) {
                System.out.print(adjMatrix[i][j] + " ");
            }
            System.out.println();
        }
        System.out.println();
        
        // 3. Adjacency List representation
        // Array of linked lists
        LinkedList<Integer>[] adjList = new LinkedList[8];
        
        // Initialize each linked list
        for (int i = 0; i < 8; i++) {
            adjList[i] = new LinkedList<>();
        }
        
        // Add edges to adjacency lists
        adjList[6-1].add(7); // 6 -> 7
        adjList[6-1].add(8); // 6 -> 8
        adjList[7-1].add(8); // 7 -> 8
        adjList[1-1].add(2); // 1 -> 2
        adjList[2-1].add(5); // 2 -> 5
        adjList[5-1].add(4); // 5 -> 4
        adjList[4-1].add(3); // 4 -> 3
        adjList[3-1].add(1); // 3 -> 1
        
        // Print adjacency list
        System.out.println("Adjacency List:");
        for (int i = 0; i < 8; i++) {
            System.out.print((i+1) + ": "); // Adjusted to show original vertex number
            for (int neighbor : adjList[i]) {
                System.out.print(neighbor + " ");
            }
            System.out.println();
        }
    }
}
```



## Implementing DFS in Java

Last week, we looked at BFS (Breadth-First Search) which first traverses all nodes directly connected to the current node, and when moving to the next node, again traverses all of its connected nodes. The search pattern resembles breadth - exploring outward in all directions simultaneously, like ripples spreading in water.

However, DFS (Depth-First Search) explores in a single direction without turning back until it reaches a dead end, only then changing direction (this direction-changing process involves backtracking). It is like following a single path as far as possible before trying alternative routes.

![Screen Shot 2025-03-24 at 9.33.50 AM](images/Screen%20Shot%202025-03-24%20at%209.33.50%20AM.png)



### Common misconception!!

We cannot know which path will be the longest before traversing the graph. **While DFS does explore "deeply" before "widely," it does not automatically find the longest path first.** 



Consider a graph with two paths from source to target:

- Path A: 0 → 1 → 2 → 3 → 4 → 5 (length 6)
- Path B: 0 → 6 → 7 → 8 (length 4)

If node 0's adjacency list (assuming we use an adjacency list to store the graph) happens to have 6 before 1, DFS will explore the shorter path B first, even though it is not the longest path.



### Backtracking in DFS: Implicit vs. Explicit

### Implicit Backtracking

- **Definition**: The natural return of control flow up the call stack that happens automatically in recursive implementations
- **How it works**: When a DFS call completes or hits a dead end, it automatically "backs up" to the previous function call
- **Always present**: Any recursive DFS implementation has this built-in behavior
- **Example**: In Number of Islands, we rely only on this implicit backtracking - when we finish exploring in one direction, execution automatically returns to try other directions

### Explicit Backtracking

- **Definition**: Deliberately undoing state changes to restore previous conditions before exploring alternative paths
- **How it works**: After exploring one path, we manually revert changes to data structures (like removing the last node from a path)
- **When needed**: Required for problems where we need to find all possible combinations, permutations, or paths
- **Example**: In All Paths From Source to Target, we explicitly use `path.remove(path.size() - 1)` to undo each step after exploring it

### Key Distinction

- Implicit backtracking is about the control flow (where execution continues)
- Explicit backtracking is about state management (restoring data to previous values)

When do we need explicit backtracking? Ask yourself these questions:

1. Do I need to find all possible solutions or just identify/count components?
2. Do I need to track a temporary state (path, partial solution) across recursive calls?
3. Can the same element appear in multiple valid solutions?
4. Do I need to restore the original state after exploration?

If you answer "yes" to any of these, you likely need backtracking. If all answers are "no," simple DFS without explicit backtracking is usually sufficient.



To keep it simple, traversing a graph (visiting all nodes) does not need explicit backtracking, finding all possible paths from a starting node needs it.



Spoiler: Number of Islands uses only implicit backtracking because we permanently mark cells as visited, while All Paths requires explicit backtracking because we need to maintain an accurate representation of the current path as we explore different possibilities.

### The Three Steps of DFS (Depth-First Search)

#### 1. Define the Recursive Function and Parameters

```java
void dfs(parameters)
```

When implementing recursion, we need to determine which parameters our recursive function requires. We can also start writing the function and add parameters as we discover we need them.

If the question requires you to find all paths, generally, DFS requires a two-dimensional array structure to store all paths, and a one-dimensional array to store a single path. For these result-storing arrays, we can define global variables to avoid having too many function parameters.



#### 2. Establish Clear Termination Conditions

Termination conditions are crucial. Many programmers encounter infinite loops, stack overflows, and other issues with DFS because they have not clearly thought through the termination conditions.

```java
if (terminationCondition) {
    storeResult;
    return;
}
```



#### 3. Process the Paths from the Current Search Node

This step typically involves a for-loop operation to traverse all nodes that can be reached from the current search node (we go as deep as possible).

```java
for (choice: all nodes connected to current node) {
    process node;
    dfs(graph, chosen node); // recursion
    (optional) explicit backtrack to undo the processing result
}
```



A common point of confusion is why we need to undo or backtrack in the DFS code framework when we have already processed nodes in the for loop if we want to find all paths starting from 1.

Consider this scenario: We can go from 1 to 2 to 3, we undo 2 -> 3 and switch to 2 -> 7? This is exactly the (implicit) backtracking process - undoing a path and exploring the next direction.

![Screen Shot 2025-03-24 at 9.46.19 AM](images/Screen%20Shot%202025-03-24%20at%209.46.19%20AM.png)



Let us now look at how to do [Number of Islands](https://leetcode.com/problems/number-of-islands/), now the DFS way before we compare the similarities and differences of DFS and BFS.



<u>DFS Solution:</u>

```java
class Solution {
    // Direction arrays for moving in 4 directions (right, down, up, left)
    private static final int[][] DIRECTIONS = {
        {1, 0},   // down
        {-1, 0},  // up
        {0, 1},   // right
        {0, -1}   // left
    };
    
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int m = grid.length;
        int n = grid[0].length;
        int count = 0;
        
        // Iterate through every cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If we find land, we found a new island
                if (grid[i][j] == '1') {
                    count++;
                    // Explore connected grid using DFS
                    dfs(grid, i, j);
                }
            }
        }
        
        return count;
    }
    
    private void dfs(char[][] grid, int x, int y) { // 1. Define the Recursive Function and Parameters
        // Mark the starting position as visited
        // by converting '1' to '0' so we don't revisit
        grid[x][y] = '0';

        // Explore in all four directions
        for (int i = 0; i < 4; i++) {
            int nextX = x + DIRECTIONS[i][0];
            int nextY = y + DIRECTIONS[i][1];
            
            // Check boundaries and if the cell is land
            if (nextX >= 0 && nextX < grid.length && 
                nextY >= 0 && nextY < grid[0].length && 
                grid[nextX][nextY] == '1') {
                
                // Mark as visited
                grid[nextX][nextY] = '0';
                
                // Continue DFS from this cell
                dfs(grid, nextX, nextY);
            }
        }
    }
  // 2. Why is there no termination condition? DFS is only called in the main method or in the dfs method when it should, in other words
  // You only call dfs() from the main function when grid[i][j] == '1'
  // You only make recursive calls on neighbors when grid[nextX][nextY] == '1'
  // In other cases, dfs() will not be called
}
```



<u>BFS Solution:</u>

```java
class Solution {
    // Directions array (down, up, right, left)
    private static final int[][] DIRECTIONS = {
        {1, 0},   // down
        {-1, 0},  // up
        {0, 1},   // right
        {0, -1}   // left
    };
    
    public int numIslands(char[][] grid) {
        // Edge case check
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int m = grid.length;
        int n = grid[0].length;
        int count = 0;
        
        // Iterate through every cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If it's land, we have found a new island
                if (grid[i][j] == '1') {
                    count++;
                    bfs(grid, i, j); // Mark all connected land
                }
            }
        }
        
        return count;
    }
    
    private void bfs(char[][] grid, int startX, int startY) {
        // Use a queue for BFS
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] {startX, startY});
        
        // Mark the starting position as visited
        // by converting '1' to '0' so we don't revisit
        grid[startX][startY] = '0';
        
        int m = grid.length;
        int n = grid[0].length;
        
        // BFS loop
        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int x = current[0], y = current[1];
            
            // Explore all four directions
            for (int[] dir : DIRECTIONS) {
                int nextX = x + dir[0];
                int nextY = y + dir[1];
                
                // Check boundaries + if it's land
                if (nextX >= 0 && nextX < m && nextY >= 0 && nextY < n
                    && grid[nextX][nextY] == '1') {
                    grid[nextX][nextY] = '0'; // Mark visited
                    queue.offer(new int[] {nextX, nextY});
                }
            }
        }
    }
}
```



What are some of the similarities and differences in the above DFS and BFS solution? :)

#### Similarities

1. Overall Structure:
   - Both scan the entire grid cell by cell
   - Both count a new island when they find an unvisited land cell
   - Both mark visited cells by changing '1' to '0'
   - Both explore in the same four directions (up, down, left, right)
   - Both have the same time complexity: O(M×N) where M is rows and N is columns
2. Island Identification Logic:
   - Both identify each distinct island exactly once
   - Both ensure every cell in an island is visited exactly once
3. Boundary Checking:
   - Both validate grid boundaries before visiting cells
   - Both check if a cell is land ('1') before processing it

## Key Differences

1. Traversal Pattern:

   - DFS:

      Explores as far as possible along one path before backtracking

     - Like following a single winding path until you hit a dead end, then backing up

   - BFS:

      Explores all cells at the same "distance" from the starting point before moving further

     - Like a ripple expanding outward from the starting point

2. Data Structures:

   - DFS:

      Uses the call stack through recursion (implicit)

     - The program's execution context remembers where to return

   - BFS:

      Uses an explicit queue data structure

     - Must manually manage the queue of cells to visit next

3. Implementation Details:

   - **DFS:** Recursive implementation
   - **BFS:** Iterative implementation with a while loop

4. Traversal Order:

   - **DFS:** Might reach the furthest cell of an island before processing nearby cells
   - **BFS:** Processes all cells at distance 1, then distance 2, etc.

5. Memory Usage Pattern:

   - DFS:

      Memory usage depends on the longest path in the island (call stack depth)

     - Could potentially cause stack overflow on very large islands

   - BFS:

      Memory usage depends on the width of the island at its widest point

     - More predictable memory usage, safer for large islands

6. Cell Marking Timing:

   - **DFS:** Marks cells as visited before exploring further (mark current, then recurse)
   - **BFS:** Marks cells when adding to the queue (prevents adding the same cell twice)

## When to Prefer Each Approach

**Prefer DFS when:**

- Code simplicity is a priority
- Islands are expected to be long and narrow
- Memory overhead of the queue would be significant

**Prefer BFS when:**

- Concerned about stack overflow with very large islands
- Want to find the shortest path between points (not relevant for this problem)
- Islands might be very wide

For this specific problem, both approaches are equally valid and produce the same result. The choice comes down to personal preference and the specific constraints of your system.

The elegant part of both solutions is how they use the grid itself as their "visited" tracking mechanism, avoiding the need for a separate data structure.



Now, let us look at a classic problem where we need explicit backtracking DFS: [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/).

```java
class Solution {
    // Class variables to track paths and results
    private List<List<Integer>> result;
    private List<Integer> path; // Current path being explored

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        result = new ArrayList<>(); // Initialize result collection
        path = new ArrayList<>();   // Initialize path list
        
        // Start with node 0 (source node)
        path.add(0);
        
        // Start DFS from node 0 to node n - 1
        dfs(graph, 0, graph.length - 1);
        
        return result;
    }
    
    private void dfs(int[][] graph, int x, int n) {
        // If we've reached the target node
        if (x == n) {
            // When a complete path is found
            // Add a copy of the current path to results
            result.add(new ArrayList<>(path)); 
            return;
        }
        
        // Explore all nodes that x points to
        for (int i : graph[x]) {
            path.add(i);           // Making a choice: Add node to current path
            dfs(graph, i, n);      // Exploring consequences of that choice: Recursive DFS
            path.remove(path.size() - 1); // Undoing the choice (removing the node) to try alternatives
            // This line is explicit backtracking!
            // Note: path.remove(index) removes the element at the specified index
        }
    }
}
```



#### (Optional) Call Stack Execution

#### First Branch: Exploring through Node 1

**Level 1: `dfs(graph, 0, 3)`**

- Path: `[0]`
- Check if at target: `0 == 3`? No
- Neighbors of 0: `[1, 2]`
- First neighbor is 1:
  - Add 1 to path → `[0, 1]`
  - Make recursive call → `dfs(graph, 1, 3)`

**Level 2: `dfs(graph, 1, 3)`**

- Call stack: `dfs(graph, 0, 3)` → `dfs(graph, 1, 3)`
- Path: `[0, 1]`
- Check if at target: `1 == 3`? No
- Neighbors of 1: `[3]`
- Only neighbor is 3:
  - Add 3 to path → `[0, 1, 3]`
  - Make recursive call → `dfs(graph, 3, 3)`

**Level 3: `dfs(graph, 3, 3)`**

- Call stack: `dfs(graph, 0, 3)` → `dfs(graph, 1, 3)` → `dfs(graph, 3, 3)`
- Path: `[0, 1, 3]`
- Check if at target: `3 == 3`? Yes!
- Add a copy of path to result: `result = [[0, 1, 3]]`
- **Return** back to where we were in `dfs(graph, 1, 3)`

**Back to Level 2: `dfs(graph, 1, 3)`**

- Call stack: `dfs(graph, 0, 3)` → `dfs(graph, 1, 3)`
- Path: `[0, 1, 3]` (before backtracking)
- We returned from recursive call for node 3
- Execute backtracking: `path.remove(path.size() - 1)` removes 3
- Path after backtracking: `[0, 1]`
- No more neighbors of 1 to process
- **Return** back to where we were in `dfs(graph, 0, 3)`

**Back to Level 1: `dfs(graph, 0, 3)`**

- Call stack: `dfs(graph, 0, 3)`
- Path: `[0, 1]` (before backtracking)
- We returned from recursive call for neighbor 1
- Execute backtracking: `path.remove(path.size() - 1)` removes 1
- Path after backtracking: `[0]`
- Move to the next neighbor of 0, which is 2

#### Second Branch: Exploring through Node 2

**Level 1 (continued): `dfs(graph, 0, 3)`**

- Path: `[0]`
- Second neighbor is 2:
  - Add 2 to path → `[0, 2]`
  - Make recursive call → `dfs(graph, 2, 3)`

**Level 2: `dfs(graph, 2, 3)`**

- Call stack: `dfs(graph, 0, 3)` → `dfs(graph, 2, 3)`
- Path: `[0, 2]`
- Check if at target: `2 == 3`? No
- Neighbors of 2: `[3]`
- Only neighbor is 3:
  - Add 3 to path → `[0, 2, 3]`
  - Make recursive call → `dfs(graph, 3, 3)`

**Level 3: `dfs(graph, 3, 3)`**

- Call stack: `dfs(graph, 0, 3)` → `dfs(graph, 2, 3)` → `dfs(graph, 3, 3)`
- Path: `[0, 2, 3]`
- Check if at target: `3 == 3`? Yes!
- Add a copy of path to result: `result = [[0, 1, 3], [0, 2, 3]]`
- **Return** back to where we were in `dfs(graph, 2, 3)`

**Back to Level 2: `dfs(graph, 2, 3)`**

- Call stack: `dfs(graph, 0, 3)` → `dfs(graph, 2, 3)`
- Path: `[0, 2, 3]` (before backtracking)
- We returned from recursive call for node 3
- Execute backtracking: `path.remove(path.size() - 1)` removes 3
- Path after backtracking: `[0, 2]`
- No more neighbors of 2 to process
- **Return** back to where we were in `dfs(graph, 0, 3)`

**Back to Level 1: `dfs(graph, 0, 3)`**

- Call stack: `dfs(graph, 0, 3)`
- Path: `[0, 2]` (before backtracking)
- We returned from recursive call for neighbor 2
- Execute backtracking: `path.remove(path.size() - 1)` removes 2
- Path after backtracking: `[0]`
- No more neighbors of 0 to process
- **Return** to main function with result: `[[0, 1, 3], [0, 2, 3]]`

##### Key Insights About the Call Stack

1. **Return Location**: When a function returns, execution picks up exactly where it left off in the parent function - specifically at the line after the recursive call.
2. **State Preservation**: The state of local variables (like loop counters) in each function call is preserved while that call is on the stack.
3. **Backtracking Timing**: The backtracking (`path.remove()`) happens immediately after a recursive DFS call returns, before processing any other neighbors.
4. **Stack Growth and Shrinking**: The call stack grows with each recursive call and shrinks with each return. At maximum depth, our call stack had three levels.
5. **Decision Tree**: The overall process resembles a decision tree, where each choice to explore a neighbor branches the path, and backtracking allows us to retrace our steps and explore other branches.

This is why backtracking is essential - it allows us to explore all possible paths by undoing our choices as we go back up the call stack, so we can make different choices and explore different paths.

## DFS vs BFS: Similarities, Differences, and When to Use Each

## Similarities

Both DFS (Depth-First Search) and BFS (Breadth-First Search) share important characteristics:

- Both systematically explore all reachable nodes in a graph
- Both mark visited nodes to avoid infinite loops
- Both have $O(V+E)$ time complexity for a graph with V vertices and E edges
- Both can identify connected components and determine reachability
- Both require data structures to track nodes to visit
- Both can solve many of the same problems, though with different efficiency trade-offs

## Key Differences

### 1. Traversal Pattern

- DFS

  : Explores as far as possible along one branch before backtracking

  - Like exploring a maze by following one path until hitting a dead end

- BFS

  : Explores all neighbors at the current distance before moving outward

  - Like a ripple expanding outward from a starting point

### 2. Data Structures

- **DFS**: Uses recursion (implicit stack) or an explicit stack
- **BFS**: Uses a queue to process nodes in order of discovery

### 3. Memory Usage

- DFS

  : Memory usage depends on maximum path depth

  - Can be efficient for deep, narrow graphs
  - Risk of stack overflow in very deep graphs

- BFS

  : Memory usage depends on maximum width of the graph

  - Can consume significant memory for wide graphs

### 4. Path Properties

- **DFS**: Doesn't guarantee shortest paths
- **BFS**: Guarantees shortest paths in unweighted graphs

### 5. Implementation Style

- **DFS**: Often recursive, making it concise for some problems
- **BFS**: Typically iterative, requiring explicit queue management

## When to Use DFS

DFS is generally better when:

1. You need all possible solutions or paths
   - Especially with backtracking to enumerate all possibilities
2. The problem naturally fits a recursive approach
   - Like traversing hierarchical structures or exploring decision trees
3. Memory is limited and graphs are deep but narrow
   - Since maximum memory depends on the longest path
4. The solution is likely far from the starting point
   - DFS can sometimes reach deep nodes faster than BFS
5. You're doing topological sorting or cycle detection
   - These algorithms naturally align with DFS

## When to Use BFS

BFS is generally better when:

1. You need the shortest path

    (in unweighted graphs)

   - Since BFS guarantees finding the shortest path first

2. Solutions are likely closer to the starting point

   - BFS finds closer nodes before distant ones

3. You need to find all nodes at a specific distance

   - BFS naturally processes nodes layer by layer

4. Working with a wide, shallow graph

   - When the graph branches heavily but isn't very deep

5. Avoiding deep recursion is important

   - BFS doesn't risk stack overflow on deep graphs

The Number of Islands problem works well with either approach since we're simply identifying connected components, not finding paths. The choice often comes down to implementation preference and the specific constraints of your environment.



## Optional Questions From Previous Semester

Q1: [Number of Islands](https://leetcode.com/problems/number-of-islands/)

Q2: [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

Q3: [Eight Queens](https://open.kattis.com/contests/y663i8/problems/8queens)

Q4: [Pokémon Ice Maze](https://open.kattis.com/contests/jq4f7h/problems/pokemon)

Q5: [N-Puzzle](https://open.kattis.com/contests/hfypyp/problems/npuzzle)

Q6: [Cut the Negativity](https://open.kattis.com/problems/cutthenegativity)

Q7: [99 Problems](https://open.kattis.com/contests/v52uty/problems/99problems)

Q8: [Sannvirði](https://open.kattis.com/contests/a9izeq/problems/sannvirdi)

Q9: [Send More Money](https://open.kattis.com/contests/yoatkk/problems/sendmoremoney)

Q10: [Counting Stars](https://open.kattis.com/contests/x8uu2t/problems/countingstars)
