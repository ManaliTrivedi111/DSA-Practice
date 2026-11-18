# 2916. Subarrays Distinct Element Sum of Squares II

# Problem Summary:

Given an integer array nums, consider every non-empty contiguous subarray. For each subarray, calculate the number of distinct elements and add the square of that count to the result. Return the sum modulo 10^9 + 7.

# Approach Used:

Use a Binary Indexed Tree (Fenwick Tree) with two internal trees to support range updates and range-sum queries in O(log n) time.

While iterating through nums, maintain the distinct counts of all subarrays ending at the current index.

For the current value a = nums[i], let j be one position after its previous occurrence, or 0 if it has not appeared before. The distinct count increases by one for every subarray ending at i whose starting index is in [j, i].

Let q be the sum of the previous distinct counts over this range. Increasing each count by one changes the sum of their squares by 2 * q + (i - j + 1). Add this change to cur, the sum of squared distinct counts for subarrays ending at i. Then add cur to the overall result, update the BIT over [j, i], and record the current index in seen.

# Steps:

1. Initialize the BIT, the seen map, and the running totals cur and res.
2. Iterate through each index i of nums.
3. Find j, one position after the previous occurrence of nums[i], or 0 if it has not appeared.
4. Query the BIT over [j, i] to get the sum q of the previous distinct counts.
5. Update cur by adding 2 * q + (i - j + 1), modulo 10^9 + 7.
6. Add cur to res, modulo 10^9 + 7.
7. Increment the BIT values over [j, i] by one and update seen.
8. Return res.

# Solution:

```
class BIT {
    int size;                                                                // T(n) = O(1), S(n) = O(1)
    long[] tree1;                                                            // T(n) = O(1), S(n) = O(n)
    long[] tree2;                                                            // T(n) = O(1), S(n) = O(n)

    public BIT(int n) {
        this.size = n;                                                       // T(n) = O(1), S(n) = O(1)
        this.tree1 = new long[n + 2];                                        // T(n) = O(n), S(n) = O(n)
        this.tree2 = new long[n + 2];                                        // T(n) = O(n), S(n) = O(n)
    }

    private int lowbit(int index) {
        return index & -index;                                               // T(n) = O(1), S(n) = O(1)
    }

    public void add(int left, int right, int delta) {
        add(left, delta);                                                    // T(n) = O(log n)
        add(right + 1, -delta);                                              // T(n) = O(log n)
    }

    private void add(int index, int delta) {
        index += 1;                                                          // T(n) = O(1)
        long rawIndex = index;                                               // T(n) = O(1), S(n) = O(1)
        while (index <= size) {                                              // T(n) = O(log n)
            tree1[index] += delta;                                           // T(n) = O(1)
            tree2[index] += rawIndex * delta;                                // T(n) = O(1)
            index += lowbit(index);                                          // T(n) = O(1)
        }
    }

    public long query(int left, int right) {
        return query(right) - query(left - 1);                               // T(n) = O(log n)
    }

    private long query(int index) {
        index += 1;                                                          // T(n) = O(1)
        long rawIndex = index + 1;                                           // T(n) = O(1), S(n) = O(1)
        if (index > size) {                                                  // T(n) = O(1)
            index = size;                                                    // T(n) = O(1)
        }
        long res = 0;                                                        // T(n) = O(1), S(n) = O(1)
        while (index > 0) {                                                  // T(n) = O(log n)
            res += rawIndex * tree1[index] - tree2[index];                   // T(n) = O(1)
            index -= lowbit(index);                                          // T(n) = O(1)
        }
        return res;                                                          // T(n) = O(1)
    }
}

class Solution {
    public int sumCounts(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        long res = 0;                                                        // T(n) = O(1), S(n) = O(1)
        long cur = 0;                                                        // T(n) = O(1), S(n) = O(1)
        BIT bit = new BIT(n + 10);                                           // T(n) = O(n), S(n) = O(n)
        Map<Integer, Integer> seen = new HashMap<>();                        // T(n) = O(1), S(n) = O(n) overall
        long mod = 1000000007;                                               // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(N * log n) overall
            int a = nums[i];                                                 // T(n) = O(1), S(n) = O(1)
            int j = seen.getOrDefault(a, -1) + 1;                            // T(n) = O(1) expected
            long q = bit.query(j, i);                                        // T(n) = O(log n)
            cur = (cur + q * 2 + i - j + 1) % mod;                           // T(n) = O(1)
            res = (res + cur) % mod;                                         // T(n) = O(1)
            bit.add(j, i, 1);                                                // T(n) = O(log n)
            seen.put(a, i);                                                  // T(n) = O(1) expected
        }
        return (int) res;                                                    // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n * log n)

# Space Complexity:
S(n) = O(n)
