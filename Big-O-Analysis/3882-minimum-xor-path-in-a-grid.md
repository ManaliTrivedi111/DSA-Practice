# 3882. Minimum XOR Path in a Grid

# Problem Summary:

Given an m x n grid, find the minimum possible bitwise XOR of all cell values along a path from the top-left cell to the bottom-right cell. Each move can be either right or down.

# Approach Used:

Use DFS with memoization to explore the possible XOR values at each cell.

The state memo[i][j][xor] records whether a particular XOR value has already been reached at cell (i, j). If the same state is encountered again, it can be skipped because exploring it again would produce the same possible results.

When the destination is reached, update the minimum XOR value. Since each cell value and XOR result is within the range [0, 1023], there are only 1024 possible XOR states per cell.

# Steps:

1. Initialize the minimum answer to Integer.MAX_VALUE.
2. Create a 3D memoization array of size m x n x 1024.
3. Start DFS from (0, 0) with an initial XOR value of 0.
4. At each cell:
   - XOR the current value with the cell's value.
   - Stop if the minimum answer is already 0 or this cell and XOR state has been visited.
   - Mark the state as visited.
   - If the destination is reached, update the minimum answer.
   - Otherwise, recursively explore the cell below and the cell to the right, if they are within the grid.
5. Return the minimum XOR value found.

# Solution:

```
class Solution {
    private int ans;                                                      // T(m, n) = O(1), S(m, n) = O(1)
    private boolean[][][] memo;                                           // T(m, n) = O(1), S(m, n) = O(1)

    public int minCost(int[][] grid) {
        int startVal = grid[0][0];                                        // T(m, n) = O(1), S(m, n) = O(1)
        final int m = grid.length;                                        // T(m, n) = O(1), S(m, n) = O(1)
        final int n = grid[0].length;                                     // T(m, n) = O(1), S(m, n) = O(1)
        ans = Integer.MAX_VALUE;                                          // T(m, n) = O(1), S(m, n) = O(1)
        memo = new boolean[m][n][1024];                                   // T(m, n) = O(m * n * 1024), S(m, n) = O(m * n * 1024)
        dfs(grid, 0, 0, 0, m, n);                                         // T(m, n) = O(m * n * 1024), S(m, n) = O(m + n)
        return ans;                                                       // T(m, n) = O(1)
    }

    private void dfs(int[][] grid, int i, int j, int xor, int m, int n) {
        xor ^= grid[i][j];                                                // T(m, n) = O(1)

        if(ans == 0 || memo[i][j][xor]) {                                 // T(m, n) = O(1)
            return;                                                       // T(m, n) = O(1)
        }

        memo[i][j][xor] = true;                                           // T(m, n) = O(1)

        if(i == m - 1 && j == n - 1) {                                    // T(m, n) = O(1)
            ans = Math.min(ans, xor);                                     // T(m, n) = O(1)
            return;                                                       // T(m, n) = O(1)
        }
        if(i + 1 < m) {                                                   // T(m, n) = O(1)
            dfs(grid, i + 1, j, xor, m, n);                               // T(m, n) = O(1) per call
        }
        if(j + 1 < n) {                                                   // T(m, n) = O(1)
            dfs(grid, i, j + 1, xor, m, n);                               // T(m, n) = O(1) per call
        }
    }
}
```

# Time Complexity:
T(m, n) = O(m * n)

# Space Complexity:
S(m, n) = O(m * n)
