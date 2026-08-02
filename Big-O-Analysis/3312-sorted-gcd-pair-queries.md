# 3312. Sorted GCD Pair Queries

# Problem Summary:
You are given an integer array nums and a list of queries. Consider every pair (nums[i], nums[j]), where 0 ≤ i < j < n.
For each pair, compute its GCD. Collect all these GCD values into an array gcdPairs and sort it in non-decreasing order.
For every query queries[i], return the value at index queries[i] in gcdPairs.

# Approach Used:
Instead of explicitly generating all O(n²) pairs, the solution counts how many pairs have each possible GCD. It first computes the frequency of every value in nums.
For every possible divisor i:
* Count how many numbers are divisible by i.
* Compute how many pairs can be formed from those numbers.

This initially counts pairs whose GCD is a multiple of i, not necessarily exactly i. To obtain the number of pairs whose GCD is exactly i, apply the inclusion-exclusion principle by subtracting the counts already assigned to multiples of i. Next, convert the counts into a prefix-sum array so that: gcdArr[i] stores the number of pairs whose GCD is less than or equal to i.

Finally, answer each query using binary search on the prefix-sum array.

# Steps:
1. Count the frequency of every value in nums.
2. Find the maximum value in the array.
3. For every possible divisor:
   * Count how many numbers are divisible by it.
   * Compute the number of possible pairs.
4. Use inclusion-exclusion to determine the number of pairs having each exact GCD.
5. Convert the counts into prefix sums.
6. For every query:
   * Binary search the prefix-sum array.
   * Return the corresponding GCD value.

# Solution:

```
class Solution {

    public int[] gcdValues(int[] nums, long[] queries) {
        final int n = nums.length;                                  // T(n) = O(1), S(n) = O(1)
        final int m = queries.length;                               // T(n) = O(1), S(n) = O(1)
        int[] ans = new int[m];                                     // T(n) = O(1), S(n) = O(m)
        int[] freq = new int[50001];                                // T(n) = O(1), S(n) = O(MAX)
        int maxNum = 0;                                             // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            if(num > maxNum) {
                maxNum = num;                                       // T(n) = O(1)
            }
            freq[num]++;                                            // T(n) = O(1)
        }

        long[] gcdArr = new long[maxNum + 1];                       // T(n) = O(1), S(n) = O(maxNum)

        for(int i = 1; i <= maxNum; i++) {
            for(int j = i; j <= maxNum; j += i) {
                gcdArr[i] += freq[j];                               // T(n) = O(1)
            }
            
            gcdArr[i] = (gcdArr[i] * (gcdArr[i] - 1)) / 2;          // T(n) = O(1)
        }

        for(int i = maxNum; i >= 1; i--) {
            for(int j = i * 2; j <= maxNum; j += i) {
                gcdArr[i] -= gcdArr[j];                             // T(n) = O(1)
            }
        }

        for(int i = 1; i <= maxNum; i++) {
            gcdArr[i] += gcdArr[i - 1];                             // T(n) = O(1)
        }

        for(int i = 0; i < m; i++) {
            ans[i] = getGcd(queries[i], gcdArr, maxNum + 1);        // T(n) = O(log maxNum)
        }
        return ans;                                                 // T(n) = O(1)
    }

    private int getGcd(long query, long[] gcdArr, int n) {
        int l = 1;                                                  // T(n) = O(1)
        int r = n - 1;                                              // T(n) = O(1)

        while(l < r) {
            int m = (l + r) / 2;                                    // T(n) = O(1)

            if(gcdArr[m] < query + 1) {
                l = m + 1;                                          // T(n) = O(1)
            } else {
                r = m;                                              // T(n) = O(1)
            }
        }
        return l;                                                   // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n + M * (log M) + q * (log M)), Where n = length of nums, and M = maximum value in nums.

# Space Complexity:
S(n) = O(M + q), where M = maximum value in nums, and q = number of queries.
