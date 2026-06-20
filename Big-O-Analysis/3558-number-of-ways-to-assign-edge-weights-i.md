# 3558. Number of Ways to Assign Edge Weights I

# Problem Summary:
You are given an undirected tree rooted at node 1. Each edge must be assigned a weight of either 1 or 2. Choose any node x located at the maximum depth from the root. The cost of the path from node 1 to x is the sum of all edge weights on that path. Return the number of ways to assign weights to the edges on this path such that the total cost is odd. Since the answer can be very large, return it modulo 10^9 + 7.

# Approach Used:
The key observation is that only the path from the root to a deepest node matters. First, the solution determines the maximum depth of the tree using DFS. Suppose the deepest node is at depth d. Then the path from node 1 to that node contains d edges. Each edge can independently receive either:
-> weight 1 (odd)
-> weight 2 (even)
For the total path cost to be odd, the path must contain an odd number of edges assigned weight 1.
For d edges:
-> Total possible assignments = 2^d
-> Exactly half produce an odd sum
-> Exactly half produce an even sum
Therefore, the number of valid assignments is: 2^(d - 1)
The solution computes the maximum depth and then uses fast exponentiation to calculate: 2^(d - 1) mod (10^9 + 7)

# Steps:
1) Count the degree of every node
2) Build an adjacency list representation of the tree
3) Start DFS from the root node
4) Compute the maximum depth reachable from node 1
5) Let d be the number of edges in the deepest root-to-node path
6) Calculate:
   -> 2^(d - 1) mod (10^9 + 7)
7) Return the result

# Solution:
```
class Solution {

    private static final int MOD = 1_000_000_007;

    public int assignEdgeWeights(int[][] edges) {

        final int n = edges.length + 1;                // T(n) = O(1), S(n) = O(1)
        int[] countEdges = new int[n + 1];             // T(n) = O(1), S(n) = O(n)
        int[] idx = new int[n + 1];                    // T(n) = O(1), S(n) = O(n)
        int[][] graph = new int[n + 1][];              // T(n) = O(1), S(n) = O(n)
        boolean[] visited = new boolean[n + 1];        // T(n) = O(1), S(n) = O(n)
        int maxDepth = 0;                              // T(n) = O(1), S(n) = O(1)

        // Loop runs n - 1 times
        for(int[] edge : edges) {
            countEdges[edge[0]]++;
            countEdges[edge[1]]++;                    
        }                                              // T(n) = O(1) per execution, total executions = O(n)

        // Loop runs n - 1 times
        for(int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];

            if(graph[u] == null) {
                graph[u] = new int[countEdges[u]];
            }

            if(graph[v] == null) {
                graph[v] = new int[countEdges[v]];
            }

            graph[u][idx[u]++] = v;
            graph[v][idx[v]++] = u;
        }                                              // T(n) = O(1) per execution, total executions = O(n)

        visited[1] = true;

        for(int v : graph[1]) {
            maxDepth = Math.max(maxDepth, calcDepth(graph, v, visited));
            
        }                                             // Total work across all DFS calls = O(n)

        return calcAns(maxDepth - 1);                 // T(n) = O(1)
    }

    private int calcDepth(int[][] graph, int u, boolean[] visited) {
        if(visited[u]) {
            return 0;
        }

        visited[u] = true;
        int depth = 0;

        for(int v : graph[u]) {
            depth = Math.max(depth, calcDepth(graph, v, visited));
        }
        return 1 + depth;
    }

    private int calcAns(int n) {
        long result = 1;
        long base = 2;

        while(n > 0) {
            if((n & 1) == 1) {
                result = (result * base) % MOD;
            }
            
            base = (base * base) % MOD;
            n >>= 1;
        }
        return (int) result;
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
