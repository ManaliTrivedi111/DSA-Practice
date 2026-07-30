# 3336. Find the Number of Subsequences With Equal GCD

# Problem Summary:
You are given an integer array nums. Find the number of pairs of non-empty subsequences (seq1, seq2) such that:
* seq1 and seq2 are disjoint.
* The GCD of seq1 is equal to the GCD of seq2.

Since the answer can be very large, return it modulo 10^9 + 7.

# Approach Used:
The solution uses dynamic programming with precomputed GCD values. First, it finds the maximum value in the array and precomputes the GCD for every pair of numbers from 0 to maxNum. This allows every future GCD lookup to be performed in constant time. 
The DP state is defined as: dp[x][y] = number of ways to process the elements seen so far such that:
* the GCD of the first subsequence is x,
* the GCD of the second subsequence is y.

Initially, both subsequences are empty, so dp[0][0] = 1.
For every number, there are three choices:
* Ignore the number.
* Add it to the first subsequence.
* Add it to the second subsequence.

Whenever a number is added, the corresponding GCD is updated using the precomputed GCD table. After processing every element, sum all states where both subsequences have the same non-zero GCD.

# Steps:
1. Find the maximum value in the array.
2. Precompute the GCD of every pair of values from 0 to maxNum.
3. Initialize the DP table with dp[0][0] = 1.
4. For every number:
   * Create a new DP table.
   * For every DP state:
     * Skip the number.
     * Add it to the first subsequence.
     * Add it to the second subsequence.
5. Replace the old DP table with the new one.
6. Sum all states where both subsequences have the same non-zero GCD.
7. Return the result.

# Solution:

```
class Solution {

    private final int MOD = 100_00_00_007;

    public int subsequencePairCount(int[] nums) {
        final int n = nums.length;                                 // T(n) = O(1)
        int maxNum = 0;                                            // T(n) = O(1)

        for(int num : nums) {
            maxNum = Math.max(maxNum, num);                        // T(n) = O(1)
        }

        int[][] gcdArr = new int[maxNum + 1][maxNum + 1];          // T(n) = O(1), S(n) = O(maxNum^2)

        for(int i = 0; i <= maxNum; i++) {

            for(int j = i; j <= maxNum; j++) {
                int gcd = gcd(i, j);                               // T(n) = O(log maxNum)
                gcdArr[i][j] = gcd;                                // T(n) = O(1)
                gcdArr[j][i] = gcd;                                // T(n) = O(1)
            }
        }

        int[][] dp = new int[maxNum + 1][maxNum + 1];              // T(n) = O(1), S(n) = O(maxNum^2)
        dp[0][0] = 1;                                              // T(n) = O(1)

        for(final int num : nums) {
            int[][] newDp = new int[maxNum + 1][maxNum + 1];       // T(n) = O(1), S(n) = O(maxNum^2)

            for(int x = 0; x <= maxNum; x++) {
                final int newX = gcdArr[x][num];                   // T(n) = O(1), S(n) = O(1)

                for(int y = 0; y <= maxNum; y++) {
                    final int newY = gcdArr[y][num];               // T(n) = O(1)

                    newDp[x][y] += dp[x][y];                       // T(n) = O(1)
                    newDp[x][y] %= MOD;                            // T(n) = O(1)

                    newDp[newX][y] += dp[x][y];                    // T(n) = O(1)
                    newDp[newX][y] %= MOD;                         // T(n) = O(1)

                    newDp[x][newY] += dp[x][y];                    // T(n) = O(1)
                    newDp[x][newY] %= MOD;                         // T(n) = O(1)
                }
            }
            dp = newDp;                                            // T(n) = O(1)
        }

        int ans = 0;                                               // T(n) = O(1)

        for(int g = 1; g <= maxNum; g++) {
            ans += dp[g][g];                                       // T(n) = O(1)
            ans %= MOD;                                            // T(n) = O(1)
        }
        return ans;                                                // T(n) = O(1)
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);                         // T(n) = O(log maxNum)
    }
}
```

# Time Complexity:
T(n) = O(m^2 * (log m) + (n * m^2)), where n = length of nums, and m = maximum value in nums

# Space Complexity:
S(n) = O(m^2), where m = maximum value in nums
