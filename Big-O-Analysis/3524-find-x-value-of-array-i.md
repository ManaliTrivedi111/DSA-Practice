# 3524. Find X Value of Array I

# Problem Summary

Given an array of positive integers nums and a positive integer k, we can choose any non-empty subarray of nums by removing a prefix and a suffix, where either the prefix or suffix may be empty.

For every possible remainder x from 0 to k - 1, count the number of non-empty subarrays whose product leaves remainder x when divided by k.

Return an array result of size k, where result[x] contains the number of such subarrays.

# Approach Used

The solution uses dynamic programming to count subarrays based on the remainder of their product modulo k.
* cnt[i] stores the number of subarrays ending at the previous position whose product has remainder i modulo k.
* For each new number num, calculate num % k.
* Every previously counted subarray can be extended by num. If its previous product had remainder i, the new product has remainder (i * mod) % k.
* tmp stores the remainder counts for all subarrays ending at the current position.
* Each single-element subarray [num] is also added separately.
* res accumulates the counts of all subarrays for each remainder.

# Steps

1. Initialize res and cnt, both of size k, with all values set to 0.
2. Iterate through every number in nums.
3. Calculate num % k.
4. Create a temporary array tmp of size k for subarrays ending at the current element.
5. For every possible previous remainder i:
   * Calculate the new product remainder (i * mod) % k.
   * Add the number of previous subarrays with remainder i to tmp[newMod].
   * Add the same count to res[newMod].
6. Add the current element itself as a single-element subarray.
7. Assign tmp to cnt so it represents the subarrays ending at the current position.
8. After processing all elements, return res.

# Solution

```
class Solution {
    public long[] resultArray(int[] nums, int k) {
        long[] res = new long[k];                                            // T(n) = O(k), S(n) = O(k)
        long[] cnt = new long[k];                                            // T(n) = O(k), S(n) = O(k)

        for(int num : nums){                                                 // T(n) = O(n)
            int mod = num % k;                                               // T(n) = O(1), S(n) = O(1)
            long[] tmp = new long[k];                                        // T(n) = O(k), S(n) = O(k)

            for(int i = 0; i < k; i++){                                      // T(n) = O(k)
                int newMod = (i * mod) % k;                                  // T(n) = O(1), S(n) = O(1)
                tmp[newMod] += cnt[i];                                       // T(n) = O(1), S(n) = O(1)
                res[newMod] += cnt[i];                                       // T(n) = O(1), S(n) = O(1)
            }
            res[mod]++;                                                      // T(n) = O(1), S(n) = O(1)
            tmp[mod]++;                                                      // T(n) = O(1), S(n) = O(1)
            cnt = tmp;                                                       // T(n) = O(1), S(n) = O(1)
        }
        return res;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```


# Time Complexity:
T(n, k) = O(n × k)

# Space Complexity:
S(n, k) = O(k)
