# 3619. Count Islands With Total Value Divisible by K

# Problem Summary:

Given an m x n grid, an island is a group of positive cells connected horizontally or vertically.

Return the number of islands whose total cell value is divisible by k.

# Approach Used:

Traverse every cell in the grid. Whenever a positive cell is found, perform Depth-First Search (DFS) to visit the entire island and calculate its total value.

During DFS, mark each visited cell as 0 so that it is not processed again. After calculating an island's sum, check whether it is divisible by k and increment the answer if it is.

# Steps:

1. Initialize the answer to 0 and get the grid dimensions.
2. Traverse every cell in the grid.
3. If a cell is positive, call DFS to calculate the total value of its island.
4. During DFS:
   - Return 0 if the current position is out of bounds or the cell is water.
   - Add the current cell's value to the island sum.
   - Mark the cell as visited by setting it to 0.
   - Recursively visit the four adjacent cells and add their returned sums.
5. If the island sum is divisible by k, increment the answer.
6. Return the answer.

# Solution:

```
class Solution {
    public int countIslands(int[][] grid, int k) {
        final int m = grid.length;                                           // T(m, n) = O(1), S(m, n) = O(1)
        final int n = grid[0].length;                                        // T(m, n) = O(1), S(m, n) = O(1)
        int ans = 0;                                                         // T(m, n) = O(1), S(m, n) = O(1)

        for(int i = 0; i < m; i++) {                                         // T(m, n) = O(m * n)
            for(int j = 0; j < n; j++) {                                     // T(m, n) = O(n) per outer iteration
                if(grid[i][j] != 0) {                                        // T(m, n) = O(1)
                    long sum = dfs(grid, i, j, m, n, k);                     // T(m, n) = O(size of island), S(m, n) = O(size of island)

                    if(sum % k == 0) {                                       // T(m, n) = O(1)
                        ans++;                                               // T(m, n) = O(1)
                    }
                }
            }
        }
        return ans;                                                          // T(m, n) = O(1)
    }

    private long dfs(int[][] grid, int i, int j, int m, int n, int k) {
        if(i < 0 || j < 0 || i == m || j == n || grid[i][j] == 0) {          // T(m, n) = O(1)
            return 0;                                                        // T(m, n) = O(1)
        }
        long sum = grid[i][j];                                               // T(m, n) = O(1), S(m, n) = O(1)
        grid[i][j] = 0;                                                      // T(m, n) = O(1)

        sum += dfs(grid, i - 1, j, m, n, k);                                 // T(m, n) = O(size of visited region)
        sum += dfs(grid, i + 1, j, m, n, k);                                 // T(m, n) = O(size of visited region)
        sum += dfs(grid, i, j - 1, m, n, k);                                 // T(m, n) = O(size of visited region)
        sum += dfs(grid, i, j + 1, m, n, k);                                 // T(m, n) = O(size of visited region)

        return sum;                                                          // T(m, n) = O(1)
    }
}
```

# Time Complexity:
T(m, n) = O(m * n)

# Space Complexity:
S(m, n) = O(m * n)

