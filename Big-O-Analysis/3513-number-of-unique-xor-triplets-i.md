# 3513. Number of Unique XOR Triplets I

# Problem Summary:

You are given an integer array nums of length n, where nums is a permutation of the numbers in the range [1, n]. 
A XOR triplet is defined as: nums[i] XOR nums[j] XOR nums[k], where i <= j <= k.
Return the number of unique XOR triplet values that can be obtained from all possible triplets.

# Approach Used:

Because nums is a permutation of [1, n], the order of the elements does not affect the set of possible XOR values. We only need to determine how many distinct values can be produced by XORing three numbers from [1, n], where elements may be reused because i <= j <= k allows i = j = k. 

The solution finds the smallest power of two that is greater than or equal to n. Let this power of two be 2^j. 
* If n is exactly a power of two, the number of unique XOR values is 2n. 
* Otherwise, the number of unique XOR values is the next power of two, 2^j. 

For n < 3, the answer is simply n.

# Steps:
1. If n < 3, return n.
2. Find the smallest j such that 2^j >= n.
3. Calculate num = 2^j.
4. If num == n, return 2n.
5. Otherwise, return num.

# Solution:

```
class Solution {

    public int uniqueXorTriplets(int[] nums) {
        final int n = nums.length;                                      // T(n) = O(1), S(n) = O(1)

        if(n < 3) {
            return n;                                                    // T(n) = O(1), S(n) = O(1)
        }

        int j = 1;                                                       // T(n) = O(1), S(n) = O(1)

        while((1 << j) < n) {
            j++;                                                         // T(n) = O(1)
        }
        int num = 1 << j;                                                // T(n) = O(1), S(n) = O(1)

        return num == n ? (n * 2) : num;                                 // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(log n)

# Space Complexity:
S(n) = O(1)
