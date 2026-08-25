# 1140. Stone Game II

# Problem Summary:

You are given an array piles representing piles of stones arranged in a row. Alice and Bob take turns taking stones, with Alice going first. On each turn, a player can take all the stones from the first X remaining piles, where 1 <= X <= 2M. After taking X piles, M becomes max(M, X). Initially, M = 1.

Both players play optimally. Return the maximum number of stones Alice can obtain.

# Approach Used:

The solution uses dynamic programming with memoization and a suffix sum array.

The state of the game is determined by two values:
* i = the index of the first remaining pile.
* m = the current value of M.

We define dp[i][m] = the maximum number of stones the current player can obtain starting from pile i with the current M = m. The suffix array is used to quickly calculate the total number of stones remaining from index i onward.

If the current player takes x piles, they receive all the stones from those piles. Instead of calculating this sum separately, we use:
stones taken = suffix[i] - suffix[i + x]

The opponent then plays optimally from i + x with the updated value of M.

The total number of remaining stones is suffix[i], so the current player's result can be written as: current player's stones = suffix[i] - opponent's best stones

Therefore, for every possible x from 1 to 2 * m: best = max(best, suffix[i] - dfs(i + x, max(m, x), piles))

If 2 * m >= n - i, the current player can take all remaining piles, so we immediately return suffix[i].

The dp array stores already calculated states so that the same state does not have to be solved again.

# Steps:

1. Create a dp table to memoize the result for each (i, m) state.
2. Create a suffix array where suffix[i] stores the total number of stones from i to the end.
3. Calculate the suffix sums from right to left.
4. Start the recursive DP with dfs(0, 1, piles).
5. If all remaining piles can be taken in the current turn, return suffix[i].
6. If the current (i, m) state has already been calculated, return the stored result.
7. Try every possible number of piles x from 1 to 2 * m.
8. Update M to max(m, x) for the next player.
9. Calculate the current player's maximum possible stones by subtracting the opponent's optimal result from the total remaining stones.
10. Store the best result in dp[i][m].
11. Return the maximum number of stones Alice can obtain.

# Solution:

```
class Solution {

    private int[][] dp;                                                      // T(n) = O(1), S(n) = O(n^2)
    private int[] suffix;                                                    // T(n) = O(1), S(n) = O(n)

    public int stoneGameII(int[] piles) {
        int n = piles.length;                                                // T(n) = O(1), S(n) = O(1)
        dp = new int[n][n];                                                  // T(n) = O(n^2), S(n) = O(n^2)
        suffix = new int[n];                                                 // T(n) = O(n), S(n) = O(n)

        suffix[n - 1] = piles[n - 1];                                        // T(n) = O(1), S(n) = O(1)

        for(int i = n - 2; i >= 0; i--) {
            suffix[i] = suffix[i + 1] + piles[i];                            // T(n) = O(1), S(n) = O(1)
        }

        return dfs(0, 1, piles);                                             // T(n) = O(n^3), S(n) = O(n^2)
    }

    private int dfs(int i, int m, int[] piles) {
        int n = piles.length;                                                // T(n) = O(1), S(n) = O(1)

        if(i >= n) {
            return 0;                                                        // T(n) = O(1), S(n) = O(1)
        }

        if(2 * m >= n - i) {
            return suffix[i];                                                // T(n) = O(1), S(n) = O(1)
        }

        if(dp[i][m] != 0) {
            return dp[i][m];                                                 // T(n) = O(1), S(n) = O(1)
        }

        int best = 0;                                                        // T(n) = O(1), S(n) = O(1)

        for(int x = 1; x <= 2 * m; x++) {
            best = Math.max(
                best,
                suffix[i] - dfs(i + x, Math.max(m, x), piles)
            );                                                               // T(n) = O(1) per iteration
        }

        dp[i][m] = best;                                                     // T(n) = O(1), S(n) = O(1)
        return best;                                                         // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^3)

# Space Complexity:
S(n) = O(n^2)
