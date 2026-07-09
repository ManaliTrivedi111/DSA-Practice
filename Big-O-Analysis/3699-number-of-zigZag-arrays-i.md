# 3699. Number of ZigZag Arrays I

# Problem Summary:
You are given three integers n, l, and r. A ZigZag array of length n must satisfy the following conditions:
* Every element lies in the range [l, r].
* No two adjacent elements are equal.
* No three consecutive elements are strictly increasing.
* No three consecutive elements are strictly decreasing.
Return the total number of valid ZigZag arrays modulo 10^9 + 7.

# Approach Used:
The solution first normalizes the value range by subtracting l from r, so only the number of distinct values matters. A dynamic programming array dp is used, where each position represents the number of valid sequences ending at a particular value rank. Initially, every value can independently form a ZigZag array of length 1, so all entries are initialized to 1.

For each additional position in the array:
* On odd iterations, the DP array is updated from left to right using prefix sums.
* On even iterations, it is updated from right to left using suffix sums.

This alternating traversal ensures that the ZigZag constraints are maintained while efficiently accumulating valid transitions without requiring an additional DP array. Finally, all DP values are summed, and the result is doubled to account for both possible starting directions (beginning with an increase or a decrease).

# Steps:
1. Normalize the value range by computing r - l.
2. Create a DP array of size r + 1 and initialize every entry to 1.
3. For each remaining position in the array:
   * If the position index is odd, update the DP array from left to right.
   * Otherwise, update it from right to left.
4. Sum all values in the DP array.
5. Multiply the result by 2 and return it modulo 10^9 + 7.

# Solution:

```
class Solution {
    private final int MOD = 1_000_000_007;

    public int zigZagArrays(int n, int l, int r) {
        r -= l;                                              // T(n) = O(1)
        int[] dp = new int[r + 1];                           // T(n) = O(1), S(n) = O(r)
        Arrays.fill(dp, 1);                                  // T(n) = O(r)
        int res = 0;                                         // T(n) = O(1), S(n) = O(1)

        for(int i = 1; i < n; i++) {
            int pre1 = 0;                                    // T(n) = O(1)
            int pre2 = 0;                                    // T(n) = O(1)

            if((i & 1) == 1) {
                for(int j = 0; j <= r; j++) {
                    pre2 = pre1 + dp[j];                     // T(n) = O(1) per iteration
                    dp[j] = pre1;                            // T(n) = O(1) per iteration
                    pre1 = pre2 % MOD;                       // T(n) = O(1) per iteration
                }
            } else {
                for(int j = r; j >= 0; j--) {
                    pre2 = pre1 + dp[j];                     // T(n) = O(1) per iteration
                    dp[j] = pre1;                            // T(n) = O(1) per iteration
                    pre1 = pre2 % MOD;                       // T(n) = O(1) per iteration
                }
            }
        }

        for(int j : dp) {
            res = (res + j) % MOD;                          // T(n) = O(1) per iteration
        }

        return (res * 2) % MOD;                             // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n * k)

where k = r - l + 1.

# Space Complexity:
S(n) = O(k)

where k = r - l + 1.
