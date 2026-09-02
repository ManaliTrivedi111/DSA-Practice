# 1563. Stone Game V

# Problem Summary:

You are given an array stoneValue representing stones arranged in a row. In each round, Alice divides the current row into two non-empty parts. Bob compares the sums of the two parts and throws away the part with the larger sum. If the sums are equal, Alice can choose which part is thrown away.

Alice's score increases by the sum of the part that remains. The game continues until only one stone remains.

Return the maximum score Alice can obtain when both players play optimally.

# Approach Used:

The solution uses dynamic programming, prefix sums, and a suffix maximum table to optimize the transitions. 

The sum array is a prefix-sum array where sum[i] stores the total value of the first i stones. Therefore, the sum of any subarray can be calculated in O(1) time.
The DP state is represented by dp[j] while processing possible starting positions. Conceptually, dp[j] stores the maximum score Alice can obtain from the relevant subarray ending at j.

For a split at position k, the left part has sum: sum[k] - sum[i]
and the right part has sum: sum[j] - sum[k]

There are two important cases:
1. Left sum <= right sum
Bob throws away the right part, so Alice keeps the left part and gains its sum. The future game continues on the left part.

2. Left sum >= right sum
Bob throws away the left part, so Alice keeps the right part and gains its sum. The future game continues on the right part.

When the two sums are equal, Alice can choose either side, so the transition can consider the better option.

The k pointer moves only forward while processing each starting position. This avoids checking every possible split independently.

The suffMax table stores maximum DP values for suffix ranges, allowing the solution to obtain the best transition for the right side in O(1) time instead of scanning all possible split positions again.

Because the possible split positions are processed efficiently using the moving k pointer and suffMax, the overall algorithm runs in O(n²) time rather than the O(n³) time of a straightforward interval DP implementation.

# Steps:

1. Create a prefix-sum array sum so that subarray sums can be calculated in O(1) time.
2. Create the dp array to store the best scores for processed intervals.
3. Create the suffMax table to store suffix maximum values used by the optimized DP transitions.
4. Process starting positions from right to left.
5. For every starting position, initialize the values needed for the current interval.
6. Move the split pointer k forward while the left sum is less than or equal to the right sum.
7. Maintain preMax to represent the best transition involving the left side.
8. Determine the appropriate split position q, including the special case where the two side sums are equal.
9. Calculate dp[j] using the best possible transition from either side.
10. Update suffMax[i][j] with the best suffix value for future transitions.
11. Return dp[n] as Alice's maximum possible score.

# Solution:

```
class Solution {
    public int stoneGameV(int[] stoneValue) {
        final int n = stoneValue.length;                                     // T(n) = O(1), S(n) = O(1)
        int[] sum = new int[n + 1];                                          // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {
            sum[i + 1] = sum[i] + stoneValue[i];                             // T(n) = O(1), S(n) = O(1)
        }

        int[] dp = new int[n + 1];                                           // T(n) = O(n), S(n) = O(n)
        int[][] suffMax = new int[n + 1][n + 1];                             // T(n) = O(n²), S(n) = O(n²)

        for(int i = n - 1; i >= 0; i--) {
            suffMax[i + 1][i + 1] = Integer.MIN_VALUE;                       // T(n) = O(1), S(n) = O(1)
            suffMax[i][i + 1] = -sum[i];                                     // T(n) = O(1), S(n) = O(1)
            int preMax = 0;                                                  // T(n) = O(1), S(n) = O(1)
            int k = i + 1;                                                   // T(n) = O(1), S(n) = O(1)

            for(int j = i + 2; j <= n; j++) {
                while(sum[k] - sum[i] <= sum[j] - sum[k]) {
                    preMax = Math.max(preMax, dp[k] + sum[k]);               // T(n) = O(1), S(n) = O(1)
                    k++;                                                     // T(n) = O(1), S(n) = O(1)
                }

                int q = (sum[k - 1] - sum[i] == sum[j] - sum[k - 1])
                        ? (k - 1)
                        : k;                                                 // T(n) = O(1), S(n) = O(1)

                dp[j] = Math.max(
                    preMax - sum[i],
                    suffMax[q][j] + sum[j]
                );                                                           // T(n) = O(1), S(n) = O(1)

                suffMax[i][j] = Math.max(
                    suffMax[i + 1][j],
                    dp[j] - sum[i]
                );                                                           // T(n) = O(1), S(n) = O(1)
            }
        }

        return dp[n];                                                        // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n²)

# Space Complexity:
S(n) = O(n²)

