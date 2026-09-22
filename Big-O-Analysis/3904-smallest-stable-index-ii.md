# 3904. Smallest Stable Index II

# Problem Summary:

You are given an integer array nums of length n and an integer k.

For each index i, its instability score is defined as: max(nums[0..i]) - min(nums[i..n - 1]), where:
* max(nums[0..i]) is the largest value among the elements from index 0 through i.
* min(nums[i..n - 1]) is the smallest value among the elements from index i through n - 1.

An index i is called stable if its instability score is less than or equal to k. The task is to return the smallest stable index. If no stable index exists, return -1.

# Approach Used:

The solution uses two passes through the array.

First, it precomputes the suffix minimum for every index. The minimum[i] array stores the smallest value in the range nums[i..n - 1]. This allows the minimum value needed for the instability score to be obtained in O(1) time during the second pass.

Second, the solution traverses the array from left to right while maintaining maxNum, which stores the maximum value encountered from index 0 through the current index.

For each index i, the instability score is calculated as: maxNum - minimum[i]

As soon as this score is less than or equal to k, the current index is the smallest stable index, so the method immediately returns i.

If no stable index is found after scanning the entire array, the method returns -1.

# Steps:

1. Store the length of the array in n.
2. Create the minimum array to store suffix minimum values.
3. Set minimum[n - 1] to the last element of nums.
4. Traverse the array from right to left and calculate the minimum value from each index to the end.
5. Initialize maxNum to Integer.MIN_VALUE.
6. Traverse the array from left to right.
7. Update maxNum with the maximum value from index 0 through the current index.
8. Calculate the instability score as maxNum - minimum[i].
9. If the score is less than or equal to k, return i.
10. If the entire array is processed without finding a stable index, return -1.

# Solution:

```
class Solution {
    public int firstStableIndex(int[] nums, int k) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        int[] minimum = new int[n];                                          // T(n) = O(1), S(n) = O(n)
        minimum[n - 1] = nums[n - 1];                                        // T(n) = O(1), S(n) = O(1)
        int maxNum = Integer.MIN_VALUE;                                      // T(n) = O(1), S(n) = O(1)

        for(int i = n - 2; i >= 0; i--) {                                    // T(n) = O(n)
            minimum[i] = Math.min(minimum[i + 1], nums[i]);                  // T(n) = O(1), S(n) = O(1)
        }

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            maxNum = Math.max(maxNum, nums[i]);                              // T(n) = O(1), S(n) = O(1)
            int score = maxNum - minimum[i];                                 // T(n) = O(1), S(n) = O(1)

            if(score <= k) {                                                 // T(n) = O(1)
                return i;                                                    // T(n) = O(1), S(n) = O(1)
            }
        }
        return -1;                                                           // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
