# 2685. Count the Number of Complete Components

# Problem Summary:
You are given an undirected graph with n vertices numbered from 0 to n - 1. Each edge is represented as: edges[i] = [u, v].
A connected component is a group of vertices where every vertex is reachable from every other vertex. A connected component is complete if every pair of vertices in the component has an edge between them.
Return the number of complete connected components in the graph.

# Approach Used:
The solution uses the Union-Find (Disjoint Set Union) data structure to identify connected components. For each connected component, it maintains:
* nodeCount – the number of vertices in the component.
* edgeCount – the number of edges in the component.

While processing each edge:
* If the two vertices belong to different components, merge the components and combine their node and edge counts.
* Otherwise, simply increase the edge count because the edge belongs to the existing component.

After all unions are complete, every root node represents one connected component. For a component with k vertices to be complete, it must contain exactly: k × (k − 1) / 2 edges.

# Steps:
1. Initialize the Union-Find structure.
2. Initialize the node count of every component to 1.
3. Process every edge:
   * Find the roots of both endpoints.
   * If they belong to different components:
     * Merge them.
     * Update the node and edge counts.
   * Otherwise, increment the edge count.
4. Traverse all component roots.
5. For each component:
   * Compute the expected number of edges.
   * If the actual edge count matches the expected value, increment the answer.
6. Return the number of complete components.

# Solution:

```
class Solution {

    private int[] parent;

    public int countCompleteComponents(int n, int[][] edges) {
        parent = new int[n];                                   // T(n) = O(1), S(n) = O(n)
        int[] nodeCount = new int[n];                          // T(n) = O(1), S(n) = O(n)
        int[] edgeCount = new int[n];                          // T(n) = O(1), S(n) = O(n)

        for(int i = 0; i < n; i++) {
            parent[i] = i;                                     // T(n) = O(1)
            nodeCount[i] = 1;                                  // T(n) = O(1)
        }

        for(int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];

            int rootU = find(u);                               // T(n) = O(1) amortized
            int rootV = find(v);                               // T(n) = O(1) amortized

            if(rootU != rootV) {
                parent[rootV] = rootU;                         // T(n) = O(1)
                nodeCount[rootU] += nodeCount[rootV];          // T(n) = O(1)
                edgeCount[rootU] += edgeCount[rootV] + 1;      // T(n) = O(1)

            } else {
                edgeCount[rootU]++;                            // T(n) = O(1)
            }
        }

        int completeComponents = 0;                            // T(n) = O(1)

        for(int i = 0; i < n; i++) {
            if(parent[i] == i) {
                int nodes = nodeCount[i];
                int totalEdges = edgeCount[i];
                if(totalEdges == (nodes * (nodes - 1)) / 2) {
                    completeComponents++;                      // T(n) = O(1)
                }
            }
        }
        return completeComponents;                             // T(n) = O(1)
    }

    private int find(int i) {
        if(parent[i] != i) {
            parent[i] = find(parent[i]);                       // T(n) = O(1) amortized
        }
        return parent[i];                                      // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(V + E), where V = number of vertices, and E = number of edges

# Space Complexity:
S(n) = O(V), where V = number of vertices
