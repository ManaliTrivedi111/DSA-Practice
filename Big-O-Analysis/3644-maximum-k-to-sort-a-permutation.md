# 3644. Maximum K to Sort a Permutation

# Problem Summary:

Given a permutation nums of the numbers from 0 to n - 1, you may swap two elements only when their bitwise AND equals a non-negative integer k.

Return the maximum value of k that allows the array to be sorted in non-decreasing order. If the array is already sorted, return 0.

# Approach Used:

For the array to be sorted, every misplaced element must be able to participate in a valid swap.

The maximum possible k is therefore the bitwise AND of all elements that are not already at their correct indices.

- If an element is already at its correct index, it does not need to be moved and is excluded from the calculation.
- If the array is already sorted, no misplaced elements are found, so the initial Integer.MAX_VALUE value remains unchanged. In that case, return 0.

# Steps:

1. Initialize res to Integer.MAX_VALUE.
2. Traverse the array.
3. If nums[i] == i, skip the element because it is already in its correct position.
4. Otherwise, update res using the bitwise AND of res and nums[i].
5. If res is still Integer.MAX_VALUE, return 0; otherwise, return res.

# Solution:

```
class Solution {
    public int sortPermutation(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        int res = Integer.MAX_VALUE;                                         // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; ++i) {                                         // T(n) = O(n)
            if(nums[i] == i) {                                               // T(n) = O(1)
                continue;                                                    // T(n) = O(1)
            }
            res &= nums[i];                                                  // T(n) = O(1)
        }
        return res == Integer.MAX_VALUE ? 0 : res;                           // T(n) = O(1)
    }
}


# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
