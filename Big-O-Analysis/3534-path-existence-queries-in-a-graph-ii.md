# 3534. Path Existence Queries in a Graph II

# Problem Summary:
You are given:
* An integer n representing the number of nodes.
* An integer array nums.
* An integer maxDiff.

An undirected edge exists between nodes i and j if: |nums[i] - nums[j]| <= maxDiff
You are also given several queries, where each query is [u, v]. For each query, return the minimum number of edges required to travel from node u to node v. If no path exists, return -1. The graph is unweighted.

# Approach Used:
The solution first sorts the nodes according to their values in nums. Using a sliding window, it determines for every sorted position the furthest index that can be reached in one move. This information forms the first level of a binary lifting (jump) table. The remaining levels are built using dynamic programming, where: jump[i][k] stores the furthest index reachable from position i using 2ᵏ jumps.

For each query:
* Convert the original node indices into their positions in the sorted array.
* Use binary lifting to find the minimum number of jumps needed to reach the destination.
* If the destination cannot be reached, return -1.

# Steps:
1. Pair every number with its original index.
2. Sort the pairs by their values.
3. Build:
   * indexMap to map original indices to sorted positions.
   * sortedNums.
4. Use a sliding window to compute the furthest reachable index for every position.
5. Build the binary lifting jump table.
6. For every query:
   * Convert the nodes to sorted positions.
   * Use binary lifting to compute the minimum number of jumps.
   * Return -1 if the destination is unreachable.

# Solution:

```
class Solution {

    public int[] pathExistenceQueries(int n, int[] nums, int maxDiff, int[][] queries) {
        final int m = queries.length;                                                   // T(n) = O(1)
        int[] ans = new int[m];                                                         // T(n) = O(1), S(n) = O(m)
        int[] indexMap = new int[n];                                                    // T(n) = O(1), S(n) = O(n)
        int[] sortedNums = new int[n];                                                  // T(n) = O(1), S(n) = O(n)
        int[][] sortedNumAndIndexes = new int[n][];                                     // T(n) = O(1), S(n) = O(n)

        for(int i = 0; i < n; i++) {
            sortedNumAndIndexes[i] = new int[]{nums[i], i};                             // T(n) = O(1)
        }

        Arrays.sort(sortedNumAndIndexes, (a, b) -> Integer.compare(a[0], b[0]));        // T(n) = O(n log n)

        for(int i = 0; i < n; i++) {
            final int sortedIndex = sortedNumAndIndexes[i][1];
            indexMap[sortedIndex] = i;                                                  // T(n) = O(1)
            sortedNums[i] = sortedNumAndIndexes[i][0];                                  // T(n) = O(1)
        }

        final int maxLevel = 32 - Integer.numberOfLeadingZeros(n) + 1;                  // T(n) = O(1)
        int[][] jump = new int[n][maxLevel];                                            // T(n) = O(1), S(n) = O(n log n)
        int right = 0;                                                                  // T(n) = O(1)

        for(int i = 0; i < n; i++) {
            while(right + 1 < n && sortedNums[right + 1] - sortedNums[i] <= maxDiff) {
                right++;                                                                // T(n) = O(1) per execution
            }
            jump[i][0] = right;                                                         // T(n) = O(1)
        }

        for(int level = 1; level < maxLevel; level++) {
            for(int i = 0; i < n; i++) {
                final int prevJump = jump[i][level - 1];
                jump[i][level] = jump[prevJump][level - 1];                             // T(n) = O(1)
            }
        }

        for(int i = 0; i < m; i++) {
            final int u = queries[i][0];
            final int v = queries[i][1];
            final int uIndex = indexMap[u];
            final int vIndex = indexMap[v];
            final int start = Math.min(uIndex, vIndex);
            final int end = Math.max(uIndex, vIndex);
            final int res = minJumps(jump, start, end, maxLevel - 1);                   // T(n) = O(log n)
            ans[i] = (res == Integer.MAX_VALUE) ? -1 : res;                             // T(n) = O(1)
        }
        return ans;                                                                     // T(n) = O(1)
    }

    private int minJumps(int[][] jump, int start, int end, int level) {
        if(start == end) {
            return 0;                                                                   // T(n) = O(1)
        }

        if(jump[start][0] >= end) { 
            return 1;                                                                   // T(n) = O(1)
        }

        if(jump[start][level] < end) {
            return Integer.MAX_VALUE;                                                   // T(n) = O(1)
        }

        int j = level;                                                                  // T(n) = O(1)

        while(j >= 0) {

            if(jump[start][j] < end) {
                break;
            }
            j--;                                                                        // T(n) = O(1) per execution
        }
        return (1 << j) + minJumps(jump, jump[start][j], end, j);                       // T(n) = O(log n)
    }
}
```

# Time Complexity:
T(n) = O(n log n + q log n), where n = number of nodes, and q = number of queries

# Space Complexity:
S(n) = O(n log n + q), where n = number of nodes, and q = number of queries
