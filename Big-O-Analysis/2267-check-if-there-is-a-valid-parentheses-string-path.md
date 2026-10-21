# 2267. Check if There Is a Valid Parentheses String Path

# Problem Summary

Given an m x n matrix of parentheses, determine whether there exists a path from the top-left cell to the bottom-right cell that:
- Moves only down or right.
- Forms a valid parentheses string.

The path contains exactly m + n - 1 cells.

# Approach Used

Use depth-first search (DFS) to explore possible paths.

The variable count represents the current balance of parentheses:
- ( increases count by 1.
- ) decreases count by 1.
- If count becomes negative, the current path cannot form a valid parentheses string.
- At the bottom-right cell, the final balance must be zero.

The solution also uses two important pruning techniques:
1. If the current balance is too large to be reduced to zero using the remaining cells, stop exploring that path.
2. Temporarily mark a visited cell with '*' so that the DFS does not revisit it while exploring the current path. The original character is restored when backtracking.

# Steps

1. Get the dimensions m and n.
2. Immediately reject the path if the first cell is ) or the last cell is (.
3. Start DFS from (0, 0) with count = 0.
4. At each cell:
   - Increase count for (.
   - Decrease count for ).
5. If count < 0, prune the current path.
6. If count is greater than the number of cells remaining, prune the current path because there are not enough cells to close all open parentheses.
7. Mark the current cell as visited.
8. Recursively explore the cell below and the cell to the right.
9. Restore the current cell when backtracking.
10. At the bottom-right cell, include its closing-parenthesis effect and return whether the final balance is zero.

# Solution

```
class Solution {
   public boolean hasValidPath(char[][] grid) {
       final int m = grid.length;                                            // T(n) = O(1), S(n) = O(1)
       final int n = grid[0].length;                                         // T(n) = O(1), S(n) = O(1)

       if(grid[0][0] == ')' || grid[m - 1][n - 1] == '(') {                  // T(n) = O(1), S(n) = O(1)
           return false;                                                     // T(n) = O(1), S(n) = O(1)
       }
       return dfs(0, 0, 0, m, n, grid);                                      // T(n) = O(2^(m + n)), S(n) = O(m + n)
   }

   private boolean dfs(int count, int i, int j, int m, int n, char[][] grid) {
       if(i == m - 1 && j == n - 1) {                                        // T(n) = O(1), S(n) = O(1)
           count--;                                                          // T(n) = O(1), S(n) = O(1)
           return count == 0;                                                // T(n) = O(1), S(n) = O(1)
       }
       final char c = grid[i][j];                                            // T(n) = O(1), S(n) = O(1)

       if(c == '(') {                                                        // T(n) = O(1), S(n) = O(1)
           count++;                                                          // T(n) = O(1), S(n) = O(1)
       }else{                                                                // T(n) = O(1), S(n) = O(1)
           count--;                                                          // T(n) = O(1), S(n) = O(1)
       }

       if(count < 0 || count > m + n - i - j) {                              // T(n) = O(1), S(n) = O(1)
           return false;                                                     // T(n) = O(1), S(n) = O(1)
       }

       grid[i][j] = '*';                                                     // T(n) = O(1), S(n) = O(1)

       if(((i + 1) < m && grid[i + 1][j] != '*' &&
           dfs(count, i + 1, j, m, n, grid)) ||
          ((j + 1) < n && grid[i][j + 1] != '*' &&
           dfs(count, i, j + 1, m, n, grid))) {                              // T(n) = O(2^(m + n)), S(n) = O(m + n)
           return true;                                                      // T(n) = O(1), S(n) = O(1)
       }
       grid[i][j] = c;                                                       // T(n) = O(1), S(n) = O(1)

       return false;                                                         // T(n) = O(1), S(n) = O(1)
   }
}
```

# Time Complexity:
T(m, n) = O(2^(m + n))


# Space Complexity:
S(m, n) = O(m + n)
