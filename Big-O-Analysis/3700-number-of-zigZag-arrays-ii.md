# 3700. Number of ZigZag Arrays II

# Problem Summary:
You are given three integers n, l, and r. A ZigZag array of length n must satisfy the following conditions:
* Every element lies in the range [l, r].
* No two adjacent elements are equal.
* No three consecutive elements are strictly increasing.
* No three consecutive elements are strictly decreasing.
Return the total number of valid ZigZag arrays modulo 10^9 + 7.

# Approach Used:
The solution models the problem as a state transition system and uses matrix exponentiation to efficiently compute the answer for large values of n. Let m = r - l + 1, the number of possible values.
Each value has two possible states:
* Down state – the previous transition was decreasing.
* Up state – the previous transition was increasing.
Thus, there are 2 × m total states. A transition matrix is constructed where each entry represents whether one state can legally transition to another while satisfying the ZigZag constraints.

The algorithm then:
* Builds the transition matrix.
* Creates an initial state vector representing arrays of length 1.
* Raises the transition matrix to the power n - 1 using fast matrix exponentiation.
* Multiplies the resulting matrix with the initial state vector.
* Sums all final states to obtain the total number of valid ZigZag arrays.

Using matrix exponentiation reduces the number of transitions from linear in n to logarithmic.

# Steps:
1. Compute the number of distinct values m = r - l + 1.
2. Create 2 × m states representing increasing and decreasing directions.
3. Build the transition matrix according to the ZigZag rules.
4. Initialize the starting state vector for arrays of length 1.
5. Compute T^(n - 1) using fast matrix exponentiation.
6. Multiply the resulting matrix with the starting vector.
7. Sum all entries of the resulting vector modulo 10^9 + 7.
8. Return the answer.

# Solution:

```
class Solution {
    private static final long MOD = 1_000_000_007L;

    private long[][] multiply(long[][] A, long[][] B) {
        int n = A.length;                                               // T(n) = O(1), S(n) = O(1)
        long[][] C = new long[n][n];                                    // T(n) = O(1), S(n) = O(n^2)

        for(int i = 0; i < n; i++) {
            for(int k = 0; k < n; k++) {
                if(A[i][k] == 0) {
                    continue;
                }

                for(int j = 0; j < n; j++) {
                    if(B[k][j] == 0) {
                        continue;
                    }

                    C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % MOD;      // T(n) = O(1) per execution
                }
            }
        }
        return C;
    }

    private long[][] power(long[][] base, long exp) {
        int n = base.length;                                            // T(n) = O(1), S(n) = O(1)
        long[][] res = new long[n][n];                                  // T(n) = O(1), S(n) = O(n^2)

        for(int i = 0; i < n; i++) {
            res[i][i] = 1;
        }

        while(exp > 0) {
            if((exp & 1) == 1) {
                res = multiply(res, base);                              // T(n) = O(n^3)
            }

            base = multiply(base, base);                                // T(n) = O(n^3)
            exp >>= 1;                                                  // T(n) = O(1)
        }
        return res;
    }

    private long[] multiply(long[][] A, long[] v) {
        int n = A.length;                                               // T(n) = O(1), S(n) = O(1)
        long[] res = new long[n];                                       // T(n) = O(1), S(n) = O(n)

        for(int i = 0; i < n; i++) {
            long cur = 0;                                               // T(n) = O(1), S(n) = O(1)

            for(int j = 0; j < n; j++) {
                if(A[i][j] == 0) {
                    continue;
                }
                cur = (cur + A[i][j] * v[j]) % MOD;                     // T(n) = O(1) per execution
            }
            res[i] = cur;                                               // T(n) = O(1) per execution
        }
        return res;                                                     // T(n) = O(1)
    }

    public int zigZagArrays(int n, int l, int r) {
        int m = r - l + 1;                                              // T(n) = O(1)
        int states = 2 * m;                                             // T(n) = O(1)

        long[][] T = new long[states][states];                          // T(n) = O(1), S(n) = O(states^2)

        for(int x = 0; x < m; x++) {
            int downState = x;                                          // T(n) = O(1), S(n) = O(1)
            int upState = x + m;                                        // T(n) = O(1), S(n) = O(1)

            for(int y = x + 1; y < m; y++) {
                T[y][upState] = 1;                                      // T(n) = O(1) per execution
            } 

            for(int y = 0; y < x; y++) {
                T[y + m][downState] = 1;                                // T(n) = O(1) per execution
            }
        }

        long[] start = new long[states];                                // T(n) = O(1), S(n) = O(states)

        for(int x = 0; x < m; x++) {
            start[x] = 1;                                               // T(n) = O(1) per execution
            start[x + m] = 1;                                           // T(n) = O(1) per execution
        }

        long[][] P = power(T, n - 1);                                   // T(n) = O(states^3 log n), S(n) = O(states^2)
        long[] finalVec = multiply(P, start);                           // T(n) = O(states^2), S(n) = O(states)
        long ans = 0;                                                   // T(n) = O(1), S(n) = O(1)

        for(long val : finalVec) {
            ans = (ans + val) % MOD;                                    // T(n) = O(1) per execution                         
        }
        return (int) ans;                                               // T(n) = O(1)
    }
}
```

# Time Complexity:
Let:
* m = r - l + 1 be the number of distinct values.
* s = 2m be the number of states.

T(n) = O(s^3 log n)

Since s = 2m, this is equivalently:

T(n) = O(m^3 log n)

# Space Complexity:
Let:
* m = r - l + 1 be the number of distinct values.
* s = 2m be the number of states.

S(n) = O(s²)

Since s = 2m, this is equivalently:

S(n) = O(m²)
