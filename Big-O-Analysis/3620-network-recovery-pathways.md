# 3620. Network Recovery Pathways

# Problem Summary:
You are given a directed acyclic graph (DAG) with n nodes numbered from 0 to n - 1.
Each edge is represented as: edges[i] = [u, v, cost], where there is a directed edge from u to v with recovery cost cost.
Some nodes may be offline. A path from node 0 to node n - 1 is valid if:
* Every intermediate node is online.
* The total recovery cost of all edges does not exceed k.

The score of a valid path is defined as the minimum edge cost along that path. Return the maximum possible path score. If no valid path exists, return -1.

# Approach Used:
The solution combines Binary Search with Dijkstra's Algorithm. The key observation is that if a path exists whose minimum edge cost is at least x, then a path also exists for every value smaller than x. This monotonic property allows binary search on the answer.
For each candidate score:
* Ignore every edge whose cost is smaller than the candidate.
* Run Dijkstra's Algorithm using only the remaining edges.
* Track the minimum total recovery cost required to reach every node.
* If the destination can be reached with total cost at most k, the candidate score is feasible.

Binary search finds the largest feasible minimum edge cost.

# Steps:
1. Build the adjacency list using only edges whose endpoints are online.
2. Record the minimum and maximum edge costs.
3. Perform binary search on the possible path score.
4. For each candidate score:
   * Run Dijkstra's Algorithm.
   * Ignore edges with cost smaller than the candidate.
   * Ensure the accumulated recovery cost never exceeds k.
5. If a valid path exists, search for a larger score.
6. Otherwise, search for a smaller score.
7. Return the maximum feasible score.

# Solution:

```
class Solution {
    private List<int[]>[] list;
    private long k;
    private int n;

    public int findMaxPathScore(int[][] edges, boolean[] online, long k) {
        this.n = online.length;                                                                  // T(n) = O(1), S(n) = O(1)
        this.k = k;                                                                              // T(n) = O(1), S(n) = O(1)
        this.list = new List[n];                                                                 // T(n) = O(1), S(n) = O(n)
        int low = Integer.MAX_VALUE;                                                             // T(n) = O(1), S(n) = O(1)
        int high = 0;                                                                            // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {
            list[i] = new ArrayList<>();                                                         // T(n) = O(1), S(n) = O(1)
        }

        for(int[] edge : edges) {
            int u = edge[0];                                                                     // T(n) = O(1), S(n) = O(1)
            int v = edge[1];                                                                     // T(n) = O(1), S(n) = O(1)
            int cost = edge[2];

            if(online[u] && online[v]) {
                low = Math.min(low, cost);                                                       // T(n) = O(1)
                high = Math.max(high, cost);                                                     // T(n) = O(1)
                list[u].add(new int[]{v, cost});                                                 // T(n) = O(1)
            }
        }

        int ans = -1;                                                                            // T(n) = O(1)

        while(low <= high) {
            int mid = low + (high - low) / 2;                                                    // T(n) = O(1)

            if(isValidPath(mid)) {
                ans = mid;                                                                       // T(n) = O(1)
                low = mid + 1;                                                                   // T(n) = O(1)
            } else {
                high = mid - 1;                                                                  // T(n) = O(1)
            }
        }
        return ans;                                                                              // T(n) = O(1)
    }

    private boolean isValidPath(int cost) {
        long[] costArr = new long[n];                                                            // T(n) = O(1), S(n) = O(n)
        Arrays.fill(costArr, Long.MAX_VALUE);                                                    // T(n) = O(n)
        PriorityQueue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[1]));     // T(n) = O(1), S(n) = O(1)
        pq.offer(new long[]{0, 0});                                                              // T(n) = O(log n)
        costArr[0] = 0;                                                                          // T(n) = O(1)

        while(!pq.isEmpty()) {
            long[] curr = pq.poll();                                                             // T(n) = O(log n)
            int u = (int) curr[0];
            long currCost = curr[1];

            if(currCost > costArr[u]) {
                continue;
            }

            if(u == n - 1) {
                return true;                                                                     // T(n) = O(1)
            }

            for(int[] map : list[u]) {
                int v = map[0];                                                                  // T(n) = O(1) 
                int edgeCost = map[1];                                                           // T(n) = O(1)

                if(edgeCost >= cost) {
                    long newCost = currCost + edgeCost;                                          // T(n) = O(1)

                    if(newCost <= k && newCost <= costArr[v]) {
                        costArr[v] = newCost;                                                    // T(n) = O(1)
                        pq.offer(new long[]{v, newCost});                                        // T(n) = O(log n)
                    }
                }
            }
        }
        return false;                                                                            // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(V^2 * (log V) * (log C)), where V = number of vertices, and C = range of edge costs

# Space Complexity:
S(n) = O(V + E), where V = number of vertices, and E = number of edges
