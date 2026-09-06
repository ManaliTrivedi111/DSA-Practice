# 3116. Kth Smallest Amount With Single Denomination Combination

# Problem Summary:

You are given an array of coin denominations and an integer k.

There are infinitely many coins of each denomination, but coins of different denominations cannot be combined. Therefore, each valid amount must be a multiple of at least one coin denomination.

The goal is to find the kth smallest amount that can be formed.

# Approach Used:

The solution uses four main ideas:

1. Remove redundant denominations
   * Sort the coins.
   * If a coin x is divisible by an already-selected smaller coin y, every multiple of x is also a multiple of y.
   * Therefore, x does not add any new possible amounts and can be removed.
2. Precompute LCMs for all subsets
   * For every subset of the remaining denominations, calculate its LCM.
   * These LCMs are used for inclusion-exclusion.
3. Count valid amounts using inclusion-exclusion
   * For a value x, count(x) calculates how many amounts from 1 through x are divisible by at least one coin.
   * For each subset:
     * Add x / LCM when the subset contains an odd number of coins.
     * Subtract x / LCM when the subset contains an even number of coins.
4. Binary search for the kth amount
   * The answer lies between k and coins[0] * k.
   * Binary search finds the smallest x for which count(x) >= k.

Let n be the number of remaining coin denominations after removing redundant coins. Since the input constraints keep n small, all 2^n subsets can be processed.

# Steps:

1. Sort the coins array.
2. Create newCoins to store only non-redundant denominations.
3. For each coin x, check whether it is divisible by any coin already in newCoins.
4. If it is not divisible by any previous coin, add it to newCoins.
5. Convert newCoins back into an integer array.
6. Let n be the number of remaining coins and calculate m = 2^n, the number of subsets.
7. Precompute the number of set bits for every subset in bitCount.
8. Precompute the LCM of every subset in lcm.
9. Set the binary-search range from k to coins[0] * k + 1.
10. During binary search, calculate how many valid amounts are <= x using count().
11. If at least k valid amounts exist, move the right boundary to x.
12. Otherwise, move the left boundary to x + 1.
13. Return the final value of l.

# Solution:

```
class Solution {
    public long findKthSmallest(int[] coins, int k) {
        Arrays.sort(coins);                                                  // T(n) = O(n log n), S(n) = O(1)
        List<Integer> newCoins = new ArrayList<>();                          // T(n) = O(1), S(n) = O(n)

        for(int x : coins) { 
            boolean flag = true;                                             // T(n) = O(1), S(n) = O(1)

            for(int y : newCoins) { 
                if(x % y == 0) { 
                    flag = false;                                            // T(n) = O(1) per execution
                    break; 
                }
            }
            if(flag) {
                newCoins.add(x);                                             // T(n) = O(1) amortized, S(n) = O(1)
            }
        }
        coins = newCoins.stream().mapToInt(i -> i).toArray();                // T(n) = O(n), S(n) = O(n)
        int n = coins.length;                                                // T(n) = O(1), S(n) = O(1)
        int m = 1 << n;                                                      // T(n) = O(1), S(n) = O(1)
        int[] bitCount = new int[m];                                         // T(n) = O(2ⁿ), S(n) = O(2ⁿ)
        long[] lcm = new long[m];                                            // T(n) = O(2ⁿ), S(n) = O(2ⁿ)
        long l = k;                                                          // T(n) = O(1), S(n) = O(1)
        long r = (long) coins[0] * k + 1;                                    // T(n) = O(1), S(n) = O(1)

        for(int mask = 1; mask < m; mask++) {
            bitCount[mask] = bitCount[mask >> 1] + (mask & 1);               // T(n) = O(1) per iteration
        }

        lcm[0] = 1;                                                          // T(n) = O(1)

        for(int mask = 1; mask < m; mask++) {
            int preMask = mask & (mask - 1);                                 // T(n) = O(1), S(n) = O(1)
            int i = Integer.numberOfTrailingZeros(mask);                     // T(n) = O(1), S(n) = O(1)
            long tmp = lcm[preMask] / gcd(lcm[preMask], coins[i]);           // T(n) = O(log C), S(n) = O(log C)

            if(tmp <= r / coins[i]) { // T(n) = O(1), S(n) = O(1)
                lcm[mask] = tmp * coins[i];                                  // T(n) = O(1)
            }else{
                lcm[mask] = r + 1;                                           // T(n) = O(1)
            }
        }
        
        while(l < r) {                                                       // T(n) = O(log(k* C))
            long x = l + (r - l) / 2;                                        // T(n) = O(1), S(n) = O(1)

            if(count(x, m, lcm, bitCount) >= k) { // T(n) = O(2ⁿ), S(n) = O(1)
                r = x;                                                       // T(n) = O(1)
            }else{
                l = x + 1;                                                   // T(n) = O(1)
            }
        }
        return l;                                                            // T(n) = O(1)
    }

    private long count(long x, int m, long[] lcm, int[] bitCount) {
        long res = 0;                                                        // T(n) = O(1), S(n) = O(1)

        for(int mask = 1; mask < m; mask++) {                                // T(n) = O(2ⁿ)
            if(lcm[mask] > x) {
                continue;                                                   
            }

            if((bitCount[mask] & 1) == 1) {
                res += x / lcm[mask];                                        // T(n) = O(1) per execution
            }else {
                res -= x / lcm[mask];                                        // T(n) = O(1) per execution
            }
        }
        return res;                                                          // T(n) = O(1)
    }

    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);                                   // T(n) = O(log C), S(n) = O(log C)
    }
}
```

# Time Complexity:
T(N, n, k, C) = O(N² + 2ⁿ * log(k * C)), where N = number of coins, n = number of non-redundant coins, c = maximum coin denomination, and k = the requested kth amount

# Space complexity:
S(n) = O(2ⁿ), where n = number of non-redundant coins
