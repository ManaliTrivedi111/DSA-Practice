# 3727. Maximum Alternating Sum of Squares

# Problem Summary

Given an integer array nums, rearrange its elements to maximize the alternating score:

arr[0]^2 - arr[1]^2 + arr[2]^2 - arr[3]^2 + ...

Return the maximum possible score.

# Approach Used

First, square every element in nums, then sort the array in ascending order.

To maximize the alternating score:
- Place the smaller half of the squared values at the negative positions.
- Place the larger half of the squared values at the positive positions.

The solution calculates the score directly by subtracting the smaller half and adding the larger half. This avoids creating another rearranged array.

# Steps

1. Store the length of nums in n.
2. Square every element in nums.
3. Sort the squared values in ascending order.
4. Subtract the smallest n / 2 values from sum.
5. Add all remaining values to sum.
6. Return sum.

# Solution

```
class Solution {
    public long maxAlternatingSum(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            nums[i] = nums[i] * nums[i];                                     // T(n) = O(1)
        }

        Arrays.sort(nums);                                                   // T(n) = O(n * log n)
        long sum = 0;                                                        // T(n) = O(1), S(n) = O(1)

        for(int i = (n / 2) - 1; i >= 0; i--) {                              // T(n) = O(n)
            sum -= nums[i];                                                  // T(n) = O(1)
        }

        for(int i = n - 1; i >= (n / 2); i--) {                              // T(n) = O(n)
            sum += nums[i];                                                  // T(n) = O(1)
        }
        return sum;                                                          // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n * log n)

# Space Complexity
S(n) = O(1) 
