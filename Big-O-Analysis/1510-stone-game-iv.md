# 1510. Stone Game IV

# Problem Summary:

You are given a pile containing n stones. Alice and Bob take turns, with Alice starting first. On each turn, a player must remove a non-zero perfect square number of stones from the pile.
A player who cannot make a move loses the game. Return true if Alice can win when both players play optimally. Otherwise, return false.

# Approach Used:

The solution uses dynamic programming. The boolean array, dp, represents whether a player can win when there are exactly i stones remaining.

So:
dp[i] = true means the current player can force a win with i stones.
dp[i] = false means the current player will lose with i stones if both players play optimally.

We start with dp[0] = false because when there are no stones left, the current player cannot make a move and therefore loses.

For every losing state i, we try removing every possible square number of stones: 1², 2², 3², ...
If i + j * j <= n, then we mark: dp[i + j * j] = true, because from that new state, the previous player has just moved to a position that can be considered winning for the player whose turn it is.

The solution only processes states where dp[i] is false, because those are the states from which the player can make moves that lead to winning states.
As soon as dp[n] becomes true, we know Alice can force a win, so we return true.

If the loop finishes without making dp[n] true, Alice cannot win, so we return false.

# Steps:

1. Create a boolean DP array of size n + 1.
2. dp[i] represents whether the state with i stones is winning.
3. Start with dp[0] = false because a player with no stones cannot make a move.
4. Traverse every state i from 0 to n.
5. Only process states where dp[i] is false.
6. Try every square number j * j that can be added without exceeding n.
7. Mark the resulting state i + j * j as true.
8. If dp[n] becomes true, return true.
9. If the loop finishes without reaching a winning state for n, return false.

# Solution:

```
class Solution {
    public boolean winnerSquareGame(int n) {
        boolean[] dp = new boolean[n + 1];                                   // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i <= n; i++) {
            if(!dp[i]) {
                for(int j = 1; i + j * j <= n; j++) {
                    dp[i + j * j] = true;                                    // T(n) = O(1), S(n) = O(1)
                }

                if(dp[n]) {
                    return true;                                             // T(n) = O(1), S(n) = O(1)
                }
            }
        }

        return false;                                                        // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n * √n)

# Space Complexity:
S(n) = O(n)
