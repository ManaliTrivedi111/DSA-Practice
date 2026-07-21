# 1301. Number of Paths with Max Score

# Problem Summary:
You are given an n × n board where:
* 'S' is the starting position (bottom-right).
* 'E' is the ending position (top-left).
* '1'–'9' represent collectible scores.
* 'X' represents an obstacle.

From 'S', you may move: Up, Left, or Up-left (diagonally). Return:
1. The maximum score that can be collected on any valid path.
2. The number of paths that achieve this maximum score, modulo 10⁹ + 7.

If no valid path exists, return [0, 0].

# Approach Used:
The solution uses Dynamic Programming with Memoized DFS. For every cell, the recursive function computes:
* The maximum score obtainable from the end cell (0,0) to the current cell.
* The number of paths that achieve this maximum score.

The recursion explores the three possible predecessor cells: Up, Left, or Up-left.
For each predecessor:
* If it produces a larger score, update both the maximum score and the path count.
* If it produces the same maximum score, add its path count.

Memoization ensures every cell is solved only once.

# Steps:
1. Convert the board into a character matrix.
2. Replace 'E' and 'S' with '0' so they do not contribute to the score.
3. Initialize:
   * DP table for maximum score.
   * Count table for the number of optimal paths.
4. Start DFS from the bottom-right cell.
5. For every cell:
   * Explore its three predecessors.
   * Ignore obstacles and invalid positions.
   * Update the maximum score and path count.
6. If no valid path exists, return [0,0].
7. Otherwise, return the maximum score and its number of paths.

# Solution:

```
class Solution {

    private final int MOD = 100_00_00_007;                        // T(n) = O(1), S(n) = O(1)
    private final int INFINITY = Integer.MAX_VALUE;               // T(n) = O(1), S(n) = O(1)
    private int[][] cnt;
    private Integer[][] dp;
    private char[][] arr;
    private int[] DIRS = {-1, -1, 0, -1};                         // T(n) = O(1), S(n) = O(1)

    public int[] pathsWithMaxScore(List<String> board) {
        final int n = board.size();                               // T(n) = O(1)
        arr = new char[n][];                                      // T(n) = O(1), S(n) = O(n^2)
        dp = new Integer[n][n];                                   // T(n) = O(1), S(n) = O(n^2)
        cnt = new int[n][n];                                      // T(n) = O(1), S(n) = O(n^2)

        for(int i = 0; i < n; i++) {
            arr[i] = board.get(i).toCharArray();                  // T(n) = O(n), S(n) = O(n)
        }

        arr[0][0] = arr[n - 1][n - 1] = '0';                      // T(n) = O(1)
        cnt[0][0] = 1;                                            // T(n) = O(1)
        int max = find(n - 1, n - 1, n);                          // T(n) = O(n^2)

        if(max < 0) {
            return new int[]{0, 0};                               // T(n) = O(1)
        }
        return new int[]{max, cnt[n - 1][n - 1]};                 // T(n) = O(1)
    }

    private int find(int i, int j, int n) {
        if(i == 0 && j == 0) {
            return 0;                                             // T(n) = O(1)
        }

        if(dp[i][j] != null) {
            return dp[i][j];                                      // T(n) = O(1)
        }

        int ans = -INFINITY;                                      // T(n) = O(1)
        int count = 0;                                            // T(n) = O(1)

        for(int k = 0; k < 3; k++) {
            int x = i + DIRS[k];
            int y = j + DIRS[k + 1];

            if(isValid(x, y, n) && arr[x][y] != 'X') {
                int sum = find(x, y, n) + arr[x][y] - '0';         // T(n) = O(1)

                if(ans == sum) {
                    count += cnt[x][y];                            // T(n) = O(1)
                    count %= MOD;                                  // T(n) = O(1)
                }else if(sum > ans) {
                    count = cnt[x][y];                             // T(n) = O(1)
                    ans = sum;                                     // T(n) = O(1)
                }
            }
        }

        cnt[i][j] = count;                                        // T(n) = O(1)
        return dp[i][j] = ans;                                    // T(n) = O(1)
    }

    private boolean isValid(int i, int j, int n) {
        return i >= 0 && j >= 0 && i < n && j < n;                // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2)

# Space Complexity:
S(n) = O(n^2)
