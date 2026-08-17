# 486. Predict the Winner

# Problem Summary:

You are given an integer array nums. Two players take turns choosing a number from either end of the array. The chosen number is added to the current player's score and removed from the array. Player 1 starts first, and both players play optimally. Return true if Player 1 can finish with a score greater than or equal to Player 2's score. Otherwise, return false.

# Approach Used:

The solution uses dynamic programming to calculate the best score difference that the current player can achieve over the other player for every subarray.
Let dp[i][j] = the maximum score difference the current player can achieve over the other player when playing with the subarray nums[i...j].

For a subarray nums[i...j], the current player has two choices:
1. Take nums[i].
   After taking nums[i], the other player plays optimally on nums[i + 1...j].
   The resulting score difference is: nums[i] - dp[i + 1][j]

2. Take nums[j].
   After taking nums[j], the other player plays optimally on nums[i...j - 1].
   The resulting score difference is: nums[j] - dp[i][j - 1]

The current player chooses whichever option produces the larger score difference: dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1])
If dp[0][n - 1] >= 0, Player 1 can finish with at least as many points as Player 2.

# Steps:

1. If the array is empty or has an even number of elements, return true.
2. Create an n x n DP table.
3. Initialize dp[i][i] with nums[i].
4. Process subarrays from shorter lengths to longer lengths.
5. For every subarray, calculate the best score difference by taking either the leftmost or rightmost number.
6. Return true if the final score difference is greater than or equal to zero.

# Solution:

```
class Solution {
    public boolean predictTheWinner(int[] nums) {
        final int n = nums.length;                                         // T(n) = O(1), S(n) = O(1)

        if(n == 0 || n % 2 == 0) {
            return true;                                                   // T(n) = O(1), S(n) = O(1)
        }

        int[][] dp = new int[n][n];                                        // T(n) = O(n^2), S(n) = O(n^2)

        for(int i = 0; i < n; i++) {
            dp[i][i] = nums[i];                                            // T(n) = O(1)
        }

        for(int d = 1; d < n; d++) {
            for(int i = 0; i + d < n; i++) {
                final int j = i + d;                                       // T(n) = O(1)

                dp[i][j] = Math.max(
                    nums[i] - dp[i + 1][j],
                    nums[j] - dp[i][j - 1]
                );                                                         // T(n) = O(1)
            }
        }
        return dp[0][n - 1] >= 0;                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2)

# Space Complexity:
S(n) = O(n^2)
