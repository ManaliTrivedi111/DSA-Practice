# 1621. Number of Sets of K Non-Overlapping Line Segments

# Problem Summary

Given n points on a 1-D plane at coordinates 0 through n - 1 and an integer k, find the number of ways to draw exactly k non-overlapping line segments such that each segment covers at least two points.

The answer must be returned modulo 10^9 + 7.

# Approach Used

The solution uses the combinatorial formula: C(n + k - 1, 2k)

It calculates this combination using the multiplicative form: C(n + k - 1, 2k) = [(n + k - 1)(n + k - 2)...(n - k)] / (2k)!

Because division is not directly possible under modular arithmetic, the solution calculates the modular inverse of the denominator using Fermat's Little Theorem: denominator^(MOD - 2) mod MOD

The modular exponentiation is performed using binary exponentiation.

# Steps

1. Set m = 2k.
2. Initialize numerator and denominator to 1.
3. Iterate from 1 to 2k:
   * Multiply numerator by n + k - i.
   * Multiply denominator by i.
   * Take both values modulo MOD.
4. Calculate the modular inverse of denominator using quickPow(denominator, MOD - 2).
5. Multiply numerator by the modular inverse of denominator.
6. Return the result modulo MOD.

# Solution

```
class Solution {
    private static final int MOD = 1_000_000_007;                            // T(n) = O(1), S(n) = O(1)

    private long quickPow(long a, long e) {
        long result = 1;                                                     // T(n) = O(1), S(n) = O(1)

        while(e > 0) {                                                       // T(n) = O(log MOD)
            if((e & 1) != 0) {                                               // T(n) = O(1)
                result = (result * a) % MOD;                                 // T(n) = O(1)
            }
            a = (a * a) % MOD;                                               // T(n) = O(1)
            e >>= 1;                                                         // T(n) = O(1)
        }
        return result;                                                       // T(n) = O(1)
    }

    public int numberOfSets(int n, int k) {
        final int m = 2 * k;                                                 // T(n) = O(1), S(n) = O(1)
        long numerator = 1;                                                  // T(n) = O(1), S(n) = O(1)
        long denominator = 1;                                                // T(n) = O(1), S(n) = O(1)
        final int sumNK = n + k;                                             // T(n) = O(1), S(n) = O(1)

        for(int i = 1; i <= m; i++) {                                        // T(n) = O(k)
            numerator = (numerator * (sumNK - i)) % MOD;                     // T(n) = O(1)
            denominator = (denominator * i) % MOD;                           // T(n) = O(1)
        }
        return (int) ((numerator * quickPow(denominator, MOD - 2)) % MOD);   // T(n) = O(log MOD)
    }
}
```

# Time Complexity:
T(n) = O(k)

# Space Complexity:
S(n) = O(1)
