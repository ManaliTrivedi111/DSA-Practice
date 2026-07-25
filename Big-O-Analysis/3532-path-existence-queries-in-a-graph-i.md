# 3532. Path Existence Queries in a Graph I

# Problem Summary:
You are given:
* An integer n representing the number of nodes.
* A sorted integer array nums.
* An integer maxDiff.

An undirected edge exists between nodes i and j if: |nums[i] - nums[j]| <= maxDiff
You are also given several queries, where each query is [u, v]. For each query, determine whether there exists a path between nodes u and v. Return a boolean array containing the answer for every query.

# Approach Used:
The solution takes advantage of the fact that nums is already sorted. Instead of constructing the graph, it identifies the contiguous ranges of nodes that belong to the same connected component.
Starting from the end of the array:
* If two consecutive values differ by at most maxDiff, they belong to the same connected component.
* Otherwise, a new connected component begins.

A range array is built where range[i] stores the last index reachable from node i within its connected component. For each query:
* Ensure u <= v.
* If range[u] >= v, then both nodes belong to the same connected component and a path exists.
* Otherwise, no path exists.

# Steps:
1. Create the range array.
2. Traverse nums from right to left.
3. Merge consecutive indices into the same connected component whenever their difference is at most maxDiff.
4. For each query:
   * Ensure u <= v.
   * Check whether v lies within the connected component starting at u.
5. Store the result and return the answer array.

# Solution:

```
class Solution {

    public boolean[] pathExistenceQueries(int n, int[] nums, int maxDiff, int[][] queries) {
        int[] range = new int[n];                              // T(n) = O(1), S(n) = O(n)
        range[n - 1] = n - 1;                                  // T(n) = O(1)
        final int m = queries.length;                          // T(n) = O(1), S(n) = O(1)
        boolean[] ans = new boolean[m];                        // T(n) = O(1), S(n) = O(m)

        for(int i = n - 2; i >= 0; i--) {
            if(nums[i + 1] - nums[i] <= maxDiff) {
                range[i] = range[i + 1];                       // T(n) = O(1)
            } else {
                range[i] = i;                                  // T(n) = O(1)
            }
        }

        for(int i = 0; i < m; i++) {
            int[] query = queries[i];
            int u = query[0];
            int v = query[1];

            if(u > v) {
                int temp = u;                                 // T(n) = O(1)
                u = v;                                        // T(n) = O(1)
                v = temp;                                     // T(n) = O(1)
            }
            ans[i] = range[u] >= v;                           // T(n) = O(1)
        }
        return ans;                                           // T(n) = O(1)
    }
}


# Time Complexity:
T(n) = O(n + q), where n = number of nodes, and q = number of queries

# Space Complexity:
S(n) = O(n + q), where n = number of nodes, and q = number of queries
