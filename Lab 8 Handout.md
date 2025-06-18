# CS2040DE Data Structures and Algorithms

# Lab 9: MST

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

- [1] Understand Trees, Spanning Trees and Minimum Spanning Trees
- [2] Learn how to implement Prim's Algorithm in Java
- [3] Learn about how to implement Kruskal's Algorithm in Java
- [4] (Optional) Get other questions from previous labs answered
- [5] (Optional) Find more practice problems to do if they wish



## Introduction to Minimum Spanning Trees

A **Minimum Spanning Tree (MST)** is a subset of edges in a connected, acyclic graph that connects all the vertices together, without any cycles and with the minimum possible total edge weight. MSTs are used in various network design problems, such as designing telecommunication networks, electrical grids, or other structures that require minimum wiring costs.

There are two popular algorithms for finding the MST of a graph: **Prim's Algorithm** and **Kruskal's Algorithm**. In this lab, we will explore both algorithms.



## Prim's Algorithm

**Prim's Algorithm** is a greedy algorithm that builds the MST by adding edges one by one, starting from a given node, while keeping the growing tree always connected. Prim's algorithm is well-suited for **dense graphs**.

### Steps of Prim's Algorithm

1. Start with an arbitrary node and mark it as part of the MST.
2. At each step, add the smallest edge that connects a node in the MST to a node not yet in the MST.
3. Repeat until all nodes are included in the MST.

### Complexity

- **Time Complexity**: O(V^2) for adjacency matrix representation. (You can also get O(E log V) with priority queue optimization (using a heap)). Below Java code is implemented using adjacency matrix.

### Implementation

```java
import java.util.Arrays;
import java.util.Scanner;

public class PrimMST {
    static final int N = 510;
    static int[][] graph = new int[N][N]; // Stores the graph
    static int[] distance = new int[N]; // Stores the distance from each node to the MST
    static boolean[] inMST = new boolean[N]; // Indicates whether a node is already in the MST
    static int[] predecessor = new int[N]; // Stores the predecessor of each node
    static int n, m; // Number of nodes and edges

    public static void prim() {
        Arrays.fill(distance, Integer.MAX_VALUE / 2); // Initialize distance array with a large number
        distance[1] = 0; // Start building the MST from node 1
        int totalWeight = 0;

        for (int i = 0; i < n; i++) { // Iterate to select n nodes to add to the MST
            int t = -1;
            for (int j = 1; j <= n; j++) { // Find the node with the shortest distance to the MST
                if (!inMST[j] && (t == -1 || distance[j] < distance[t])) {
                    t = j;
                }
            }

            // If the selected node is isolated, print "impossible" and exit
            if (distance[t] == Integer.MAX_VALUE / 2) {
                System.out.println("impossible");
                return;
            }

            inMST[t] = true; // Add the selected node to the MST
            totalWeight += distance[t];

            // Update the distance of nodes not yet in the MST
            for (int j = 1; j <= n; j++) {
                if (!inMST[j] && graph[t][j] < distance[j]) { // If the distance from t to j is smaller, update it
                    distance[j] = graph[t][j];
                    predecessor[j] = t; // Set the predecessor of node j to t
                }
            }
        }

        System.out.println(totalWeight);
    }

    public static void printMSTEdges() { // Output all edges in the MST
        for (int i = 2; i <= n; i++) { // Since we have n nodes, there will be n-1 edges
            System.out.println(i + " " + predecessor[i]); // i is the node, predecessor[i] is its predecessor
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        n = scanner.nextInt();
        m = scanner.nextInt();

        for (int[] row : graph) {
            Arrays.fill(row, Integer.MAX_VALUE / 2); // Initialize all distances to a large value
        }

        for (int i = 0; i < m; i++) {
            int a = scanner.nextInt();
            int b = scanner.nextInt();
            int weight = scanner.nextInt();
            graph[a][b] = graph[b][a] = Math.min(graph[a][b], weight); // Store the minimum weight between nodes a and b
        }

        prim(); // Calculate the MST
        printMSTEdges(); // Uncomment to print the MST edges
    }
}
/*
Sample input:
7 9
1 6 10
1 2 28
2 7 14
2 3 16
3 4 12
4 7 18
4 5 22
5 7 24
5 6 25
*/
```



**Question**: Can we change the updating step to the following step:

```java
for (int j = 1; j <= n; j ++){
    distance[j] = Math.min(distance[j], graph[t][j]);
    predecessor[j] = t;
}
```

Then, can we first update the distance of nodes not yet in the MST based on the node t, and then update the totalWeight += distance[t]?



If we are dealing with sparse graphs, we can also use a **heap (priority queue)** to optimize the Prim's algorithm. But the following Kruskal's algorithm could be easier.



## Kruskal's Algorithm

**Kruskal's Algorithm** is another greedy algorithm that finds the MST by considering all edges in increasing order of their weights and adding them to the MST, ensuring that no cycle is formed.

### Steps of Kruskal's Algorithm

1. Sort all edges in non-decreasing order of their weights.
2. Initialize a forest with each vertex as its own component.
3. For each edge in sorted order, add the edge if it connects two different components, merge them.
4. Continue until all vertices are connected.

### Complexity

- **Time Complexity**: O(E log E) due to sorting the edges, and merging step costs almost O(E) time if union-find with path compression is used.

### Implementation

```java
import java.util.*;

public class KruskalMST {
    static final int N = 100010;
    static int[] parent = new int[N]; // Union-Find parent array

    static class Edge implements Comparable<Edge> {
        int from, to, weight;
        Edge(int from, int to, int weight) {
            this.from = from;
            this.to = to;
            this.weight = weight;
        }

        @Override
        public int compareTo(Edge other) {
            return Integer.compare(this.weight, other.weight);
        }
    }

    static List<Edge> edges = new ArrayList<>();
    static int totalWeight = 0;
    static int edgeCount = 0;
    static int n, m;

    // Find function with path compression for Union-Find
    static int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); 
          // Path compression: parent[x] = find(parent[x]) flattens the structure. After finding the ultimate root of x, we store that root directly into parent[x]. This greatly speeds up future lookups.
        }
        return parent[x];
    }

    // Kruskal's algorithm to find the MST
    static void kruskal() {
        Collections.sort(edges); // Sort edges by weight

        for (Edge edge : edges) {
            int rootA = find(edge.from); // Find the root of the start vertex.
            int rootB = find(edge.to); // Find the root of the end vertex.
						
          	/*
            Pick edges in increasing order of cost.
						Only include an edge if it connects two previously unconnected parts of the growing MST.
            */
          
            // If the nodes are not in the same set, include this edge in the MST (no cycle formed)
            if (rootA != rootB) {
                totalWeight += edge.weight;
                parent[rootA] = rootB; // Union operation: the root of A’s set now points to the root of B’s set
                edgeCount++;
            }
            // else, skip (it would form a cycle)
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        n = scanner.nextInt(); // Number of nodes
        m = scanner.nextInt(); // Number of edges

        // Initialize Union-Find parent array
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
        }

        // Read each edge
        for (int i = 0; i < m; i++) {
            int from = scanner.nextInt();
            int to = scanner.nextInt();
            int weight = scanner.nextInt();
            edges.add(new Edge(from, to, weight));
        }

        // Run Kruskal's algorithm
        kruskal();

        // If the number of edges in the MST is less than n - 1, the graph is not connected
        if (edgeCount < n - 1) {
            System.out.println("impossible");
        } else {
            System.out.println(totalWeight);
        }
    }
}
```



## Optional Questions from Previous Semester

Q1: [Island Hopping](https://open.kattis.com/contests/zi9fp2/problems/islandhopping)

Q2: [Communications Satellite](https://open.kattis.com/contests/zpqqpv/problems/communicationssatellite)

Q3: [Arctic Network](https://open.kattis.com/contests/hd93j7/problems/arcticnetwork)



## Optional Union-Find Tutorial

### Overview

The **Union-Find** data structure, also known as the **Disjoint Set Union (DSU)**, is used to keep track of a set of elements partitioned into a number of disjoint (non-overlapping) subsets. It provides efficient operations for:

1. **Finding** the representative (or root) of the set containing a particular element.
2. **Union** of two sets.

Union-Find is often used in network connectivity, finding connected components in a graph, and Kruskal's algorithm for finding the MST.

### Operations

1. **Find(x)**: This operation returns the representative (root) of the set containing element `x`. To optimize this operation, we use **path compression**, which makes nodes in the path from `x` to the root point directly to the root, flattening the structure and speeding up future operations.
2. **Union(x, y)**: This operation unites the sets containing elements `x` and `y`. To keep the structure flat, we use **union by rank**, where the root of the smaller tree is made a child of the root of the larger tree.

### Implementation in Java

```java
public class UnionFind {
    private int[] parent;
    private int[] rank;

    public UnionFind(int size) {
        parent = new int[size];
        rank = new int[size];
        for (int i = 0; i < size; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    // Find with path compression
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }

    // Union by rank
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);

        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
}
```

### Example Usage

```
public static void main(String[] args) {
    UnionFind uf = new UnionFind(10);

    // Initially, all elements are in their own sets
    uf.union(1, 2);
    uf.union(2, 3);
    System.out.println(uf.find(1) == uf.find(3)); // true

    uf.union(4, 5);
    System.out.println(uf.find(1) == uf.find(5)); // false

    uf.union(3, 4);
    System.out.println(uf.find(1) == uf.find(5)); // true
}
```

In this example, we create a union-find data structure with 10 elements. We then perform union operations to connect elements and use the find operation to determine if elements are in the same set.

### Complexity

- **Time Complexity**: With path compression and union by rank, the time complexity for each operation is nearly `O(1)` in practice. More formally, it is `O(α(n))`, where `α` is the **inverse Ackermann function**, which grows very slowly.
- **Path compression** is a more aggressive optimization and results in better performance, which is why it is often sufficient on its own.



## Optional: Borůvka's Algorithm for Minimum Spanning Tree

**Borůvka's Algorithm** is another greedy approach for finding a Minimum Spanning Tree (MST) of a graph. This algorithm is particularly well-suited for **parallel computing** because of its independent processing of multiple edges.

### Overview

Borůvka's algorithm repeatedly selects the cheapest edge connecting each component of the graph to any other component until only one component remains. This process continues until the MST is formed, merging components iteratively in a greedy manner.

**Advantages**:

- Well-suited for **parallelization** since each component finds its minimum edge independently.
- Works well for dense graphs or graphs where edge weights are highly varied.

### Steps of Borůvka's Algorithm

1. Start with each vertex as its own component.
2. **For each component**:
   - Find the **minimum-weight edge** connecting the component to any other component.
   - Add the selected edges to the MST and **merge the components**.
3. Repeat until there is only one component left.

### Complexity

- **Time Complexity**: The worst-case time complexity is `O(E log V)`.
- This complexity is similar to Kruskal's algorithm due to the merging of components and the need to repeatedly find the smallest edge.

### Implementation in Java

Below is a Java implementation of Borůvka's Algorithm for finding an MST. The implementation uses a **Union-Find** data structure to manage the merging of components.

```
import java.util.*;

public class BoruvkaMST {
    static class Edge {
        int from, to, weight;
        Edge(int from, int to, int weight) {
            this.from = from;
            this.to = to;
            this.weight = weight;
        }
    }

    static final int N = 10010;
    static List<Edge> edges = new ArrayList<>();
    static int[] parent = new int[N];
    static int[] rank = new int[N];
    static int n, m;
    static int totalWeight = 0;

    // Find function with path compression for Union-Find
    static int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    // Union function with rank to keep tree flat
    static void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }

    // Borůvka's algorithm to find the MST
    static void boruvka() {
        int numComponents = n;
        totalWeight = 0;

        while (numComponents > 1) {
            Edge[] cheapest = new Edge[n + 1];

            // Find the cheapest edge for each component
            for (Edge edge : edges) {
                int rootFrom = find(edge.from);
                int rootTo = find(edge.to);
                if (rootFrom != rootTo) {
                    if (cheapest[rootFrom] == null || edge.weight < cheapest[rootFrom].weight) {
                        cheapest[rootFrom] = edge;
                    }
                    if (cheapest[rootTo] == null || edge.weight < cheapest[rootTo].weight) {
                        cheapest[rootTo] = edge;
                    }
                }
            }

            // Add the cheapest edges to the MST and merge components
            for (int i = 1; i <= n; i++) {
                if (cheapest[i] != null) {
                    Edge edge = cheapest[i];
                    int rootFrom = find(edge.from);
                    int rootTo = find(edge.to);
                    if (rootFrom != rootTo) {
                        union(rootFrom, rootTo);
                        totalWeight += edge.weight;
                        numComponents--;
                    }
                }
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        n = scanner.nextInt(); // Number of nodes
        m = scanner.nextInt(); // Number of edges

        // Initialize Union-Find
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
            rank[i] = 0;
        }

        // Read edges
        for (int i = 0; i < m; i++) {
            int from = scanner.nextInt();
            int to = scanner.nextInt();
            int weight = scanner.nextInt();
            edges.add(new Edge(from, to, weight));
        }

        // Run Borůvka's algorithm
        boruvka();

        // Output the total weight of the MST
        System.out.println(totalWeight);
    }
}
```

### Key Differences Compared to Prim's and Kruskal's Algorithms

1. **Parallel Processing**:
   - Borůvka's algorithm finds the minimum edge for each component independently, making it more suitable for parallel computation compared to Prim's or Kruskal's algorithms.
2. **Merging Components**:
   - While Kruskal's algorithm adds edges in sorted order, Borůvka's algorithm focuses on merging multiple components at each stage, which may lead to fewer iterations in certain cases.
3. **Choice of Algorithm**:
   - **Prim's Algorithm** is typically better for dense graphs due to its adjacency matrix or heap-based approach.
   - **Kruskal's Algorithm** is suitable for sparse graphs, as it sorts edges and uses union-find for cycle detection.
   - **Borůvka's Algorithm** can be a good choice when the graph is distributed across different processors, as its parallel nature helps in merging components effectively.

