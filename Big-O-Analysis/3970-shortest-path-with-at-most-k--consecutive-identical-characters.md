# 3970. Shortest Path With at Most K Consecutive Identical Characters

# Problem Summary:

Given a directed weighted graph and a character label for each node, find the minimum total edge weight of a path from node 0 to node n - 1 such that the path's node labels never contain more than k consecutive identical characters.

Nodes and edges may be revisited. Return -1 if no valid path exists.

# Approach Used:

Use Dijkstra's algorithm on an expanded state graph.

A state is represented by (node, cnt), where node is the current node and cnt is the number of consecutive occurrences of the current node's label at the end of the path.

The distance array dis[node][cnt] stores the minimum cost to reach that state.

When traversing an edge from x to y, increment cnt if the two node labels are equal; otherwise, reset it to 1. Skip the transition if the new count exceeds k.

A priority queue processes states in increasing order of distance. The first time a valid state at node n - 1 is removed from the queue, its distance is the minimum possible answer.

# Steps:

1. Build an adjacency list containing each node's outgoing edges and their weights.
2. Initialize dis with dimensions n x (k + 1) and fill it with Integer.MAX_VALUE.
3. Set dis[0][1] to 0 and insert the initial state (0, 0, 1) into the priority queue.
4. While the priority queue is not empty:
   - Remove the state with the smallest distance.
   - Skip it if its distance is greater than the currently recorded distance.
   - If the current node is the destination, return its distance.
   - Examine each outgoing edge and calculate the next consecutive-label count.
   - If the count does not exceed k and the new distance is smaller, update the distance and add the new state to the queue.
5. If the queue becomes empty without reaching the destination, return -1.

# Solution:

```
import java.util.*;

class Solution {

    public int shortestPath(int n, int[][] edges, String labels, int k) {
        List<int[]>[] g = new ArrayList[n];                                  // T(n, E) = O(n), S(n, E) = O(n)
        Arrays.setAll(g, i -> new ArrayList<>());                            // T(n, E) = O(n), S(n, E) = O(n)
        long[][] dis = new long[n][k + 1];                                   // T(n, E, k) = O(n * k), S(n, E, k) = O(n * k)

        for(int[] e : edges) {                                               // T(n, E) = O(E)
            g[e[0]].add(new int[] { e[1], e[2] });                           // T(n, E) = O(1) amortized, S(n, E) = O(1) per edge
        }

        for(long[] row : dis) {                                              // T(n, E, k) = O(n * k)
            Arrays.fill(row, Integer.MAX_VALUE);                             // T(n, E, k) = O(k) per row
        }

        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> Long.compare(a[0], b[0])); // T(n, E, k) = O(1), S(n, E, k) = O(1)
        dis[0][1] = 0;                                                       // T(n, E, k) = O(1)
        pq.offer(new long[] { 0, 0, 1 });                                    // T(n, E, k) = O(log(n * k)), S(n, E, k) = O(1) per entry

        while(!pq.isEmpty()) {                                               // T(n, E, k) = O(k * E * log(n * k)) overall
            long[] cur = pq.poll();                                          // T(n, E, k) = O(log(n * k))
            long d = cur[0];                                                 // T(n, E, k) = O(1)
            int x = (int) cur[1];                                            // T(n, E, k) = O(1)
            int cnt = (int) cur[2];                                          // T(n, E, k) = O(1)

            if(d > dis[x][cnt]) {                                            // T(n, E, k) = O(1)
                continue;                                                    // T(n, E, k) = O(1)
            }
            if(x == n - 1) {                                                 // T(n, E, k) = O(1)
                return (int) d;                                              // T(n, E, k) = O(1)
            }

            for(int[] e : g[x]) {                                            // T(n, E, k) = O(outdegree of x) per state
                int y = e[0];                                                // T(n, E, k) = O(1)
                int w = e[1];                                                // T(n, E, k) = O(1)
                int newCnt = labels.charAt(x) == labels.charAt(y) ? cnt + 1 : 1; // T(n, E, k) = O(1)

                if(newCnt > k) {                                             // T(n, E, k) = O(1)
                    continue;                                                // T(n, E, k) = O(1)
                }

                long nd = d + w;                                             // T(n, E, k) = O(1)

                if(nd < dis[y][newCnt]) {                                    // T(n, E, k) = O(1)
                    dis[y][newCnt] = nd;                                     // T(n, E, k) = O(1)
                    pq.offer(new long[] { nd, y, newCnt });                  // T(n, E, k) = O(log(n * k)), S(n, E, k) = O(1) per entry
                }
            }
        }
        return -1;                                                           // T(n, E, k) = O(1)
    }
}
```

# Time Complexity:
T(n, E, k) = O(k * E * log(n * k)), where n = number of nodes, E = number of edges, and k = maximum allowed number of consecutive identical labels

# Space Complexity:
S(n, E, k) = O(n * k + k * E), where n = number of nodes, E = number of edges, and k = maximum allowed number of consecutive identical labels

