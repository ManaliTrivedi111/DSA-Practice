# 3756. Concatenate Non-Zero Digits and Multiply by Sum II

# Problem Summary:
You are given:
* A string s consisting of digits.
* A list of queries, where each query is [l, r].

For every query:
1. Extract the substring s[l..r].
2. Concatenate all non-zero digits (in their original order) to form a number x.
3. Compute the sum of the digits in x.
4. Return x * sum modulo 10^9 + 7.

# Approach Used:
The solution preprocesses the string using prefix arrays so that every query can be answered in constant time. Four prefix arrays are maintained:
* sum[i] – prefix sum of digits.
* count[i] – number of non-zero digits seen so far.
* num[i] – number formed by concatenating all non-zero digits up to index i (modulo 10^9 + 7).
* pow[i] – powers of 10 modulo 10^9 + 7.

For each query:
* Compute the digit sum using the prefix sum array.
* Compute the concatenated number by removing the contribution of the prefix before l.
* Multiply the two values modulo 10^9 + 7.

Since all required information is precomputed, every query is answered in O(1) time.

# Steps:
1. Convert the string into a character array.
2. Build:
   * Prefix digit sums.
   * Prefix counts of non-zero digits.
   * Prefix concatenated numbers.
   * Powers of 10 modulo 10^9 + 7.
3. For every query:
   * Compute the digit sum of the substring.
   * Compute the concatenated non-zero number using the prefix arrays.
   * Multiply the two values modulo 10^9 + 7.
4. Return the answer array.

# Solution:

```
class Solution {

    private final int MOD = 100_00_00_007;                          // T(n) = O(1), S(n) = O(1)

    public int[] sumAndMultiply(String s, int[][] queries) {
        char[] str = s.toCharArray();                               // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                                   // T(n) = O(1), S(n) = O(1)
        final int m = queries.length;                               // T(n) = O(1), S(n) = O(1)
        int[] sum = new int[n];                                     // T(n) = O(1), S(n) = O(n)
        int[] count = new int[n];                                   // T(n) = O(1), S(n) = O(n)
        long[] num = new long[n];                                   // T(n) = O(1), S(n) = O(n)
        long[] pow = new long[n + 1];                               // T(n) = O(1), S(n) = O(n)
        num[0] = sum[0] = str[0] - '0';                             // T(n) = O(1)
        count[0] = (str[0] - '0' == 0) ? 0 : 1;                     // T(n) = O(1)
        pow[0] = 1;                                                 // T(n) = O(1)
        int[] ans = new int[m];                                     // T(n) = O(1), S(n) = O(m)

        for(int i = 1; i <= n; i++) {
            pow[i] = (pow[i - 1] * 10) % MOD;                       // T(n) = O(n)
        }

        for(int i = 1; i < n; i++) {
            int digit = str[i] - '0';
            sum[i] = sum[i - 1] + digit;                            // T(n) = O(1)

            if(digit == 0) {
                num[i] = num[i - 1];                                // T(n) = O(1)
                count[i] = count[i - 1];                            // T(n) = O(1)
            } else {
                count[i] = count[i - 1] + 1;                        // T(n) = O(1)
                num[i] = ((num[i - 1] * 10) + digit) % MOD;         // T(n) = O(1)
            }
        }

        for(int i = 0; i < m; i++) {
            int l = queries[i][0];
            int r = queries[i][1];

            if(l == 0) {
                ans[i] = (int)((num[r] * sum[r]) % MOD);            // T(n) = O(1)
            } else {
                int currSum = sum[r] - sum[l - 1];                  // T(n) = O(1)
                long currNum = (num[r] - (num[l - 1] *
                    pow[count[r] - count[l - 1]])) % MOD;           // T(n) = O(1)

                if(currNum < 0) {
                    currNum += MOD;                                 // T(n) = O(1)
                }
                ans[i] = (int)((currNum * currSum) % MOD);          // T(n) = O(1)
            }
        }
        return ans;                                                 // T(n) = O(1)
    }
}


# Time Complexity:
T(n) = O(n + q), where n = length of the string, and q = number of queries

# Space Complexity:
S(n) = O(n + q), where n = length of the string, and q = number of queries
