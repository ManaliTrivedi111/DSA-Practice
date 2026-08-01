# 3867. Sum of GCD of Formed Pairs

# Problem Summary:
You are given an integer array nums. Construct an array prefixGcd such that for every index i:
* mxi = maximum value among nums[0...i].
* prefixGcd[i] = gcd(nums[i], mxi).

After constructing prefixGcd:
1. Sort it in non-decreasing order.
2. Pair the smallest unpaired element with the largest unpaired element.
3. Continue until all possible pairs are formed.
4. Compute the GCD of every pair.
5. If the array has an odd length, ignore the middle element.

Return the sum of the GCDs of all formed pairs.

# Approach Used:
The solution first constructs the prefixGcd array while maintaining the maximum value seen so far. For every element:
* Update the running maximum.
* Compute the GCD of the current element and the running maximum.

Once the prefixGcd array is built, it is sorted.

Using two pointers:
* One starts at the beginning (smallest value).
* The other starts at the end (largest value).

Each pair contributes the GCD of its two values to the answer. The pointers move inward until all pairs have been processed.

# Steps:
1. Initialize the first element of prefixGcd with the first element of nums.
2. Maintain the maximum value seen so far.
3. For every remaining element:
   * Update the running maximum.
   * Compute its GCD with the running maximum.
4. Sort the prefixGcd array.
5. Use two pointers to pair the smallest and largest remaining elements.
6. Add the GCD of each pair to the answer.
7. Return the final sum.

# Solution:

```
class Solution {

    public long gcdSum(int[] nums) {

        final int n = nums.length;                             // T(n) = O(1), S(n) = O(1)
        long sum = 0;                                          // T(n) = O(1), S(n) = O(1)
        int[] prefixGcd = new int[n];                          // T(n) = O(1), S(n) = O(n)
        prefixGcd[0] = nums[0];                                // T(n) = O(1)
        int maxNum = nums[0];                                  // T(n) = O(1)

        for(int i = 1; i < n; i++) {
            maxNum = Math.max(maxNum, nums[i]);                // T(n) = O(1)
            prefixGcd[i] = gcd(nums[i], maxNum);               // T(n) = O(log M)
        }

        Arrays.sort(prefixGcd);                                // T(n) = O(n log n)
        int i = 0;                                             // T(n) = O(1)
        int j = n - 1;                                         // T(n) = O(1)

        while(i < j) {
            sum += gcd(prefixGcd[i], prefixGcd[j]);            // T(n) = O(log M)
            i++;                                               // T(n) = O(1)
            j--;                                               // T(n) = O(1)
        }
        return sum;                                            // T(n) = O(1)
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);                     // T(n) = O(log M)
    }
}
```

# Time Complexity:
T(n) = O(n log n)

# Space Complexity:
S(n) = O(n)
