# 1658. Minimum Operations to Reduce X to Zero

# Problem Summary

You are given an integer array nums and an integer x. In one operation, you can remove either the leftmost or the rightmost element from nums and subtract its value from x.

The goal is to find the minimum number of operations required to reduce x to exactly 0. If it is impossible, return -1.

# Approach Used

Instead of directly finding the elements removed from the two ends, we find the longest contiguous subarray whose sum is totalSum - x.

If the remaining subarray has sum totalSum - x, then the elements removed from the two ends have sum x. Therefore: minimum operations = (n - maximum length of such a subarray)

Because all elements are positive, a sliding-window approach can find the longest subarray with the target sum in linear time.

# Steps

1. Calculate the total sum of all elements in nums.
2. Let target = totalSum - x. The remaining subarray must have this sum.
3. If target < 0, it is impossible to reduce x to zero.
4. If target == 0, all elements must be removed, so the answer is n.
5. Use a sliding window with left and right pointers to find the longest subarray whose sum equals target.
6. Whenever the current window sum exceeds target, move left forward until the sum is at most target.
7. Track the maximum length of a window whose sum equals target.
8. If no such subarray exists, return -1.
9. Otherwise, return n - maxLength.

# Solution

```
class Solution {
    public int minOperations(int[] nums, int x) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        int totalSum = 0;                                                    // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {                                                // T(n) = O(n)
            totalSum += num;                                                 // T(n) = O(1), S(n) = O(1)
        }

        int target = totalSum - x;                                           // T(n) = O(1), S(n) = O(1)

        if(target < 0) {                                                     // T(n) = O(1), S(n) = O(1)
            return -1;                                                       // T(n) = O(1), S(n) = O(1)
        }

        if(target == 0) {                                                    // T(n) = O(1), S(n) = O(1)
            return n;                                                        // T(n) = O(1), S(n) = O(1)
        }

        int left = 0;                                                        // T(n) = O(1), S(n) = O(1)
        int sum = 0;                                                         // T(n) = O(1), S(n) = O(1)
        int maxLength = -1;                                                  // T(n) = O(1), S(n) = O(1)

        for(int right = 0; right < n; right++) {                             // T(n) = O(n)
            sum += nums[right];                                              // T(n) = O(1), S(n) = O(1)

            while(sum > target) {                                            // T(n) = O(n)
                sum -= nums[left];                                           // T(n) = O(1), S(n) = O(1)
                left++;                                                      // T(n) = O(1), S(n) = O(1)
            }

            if(sum == target) {                                              // T(n) = O(1), S(n) = O(1)
                maxLength = Math.max(maxLength, right - left + 1);           // T(n) = O(1), S(n) = O(1)
            }
        }

        if(maxLength == -1) {                                                // T(n) = O(1), S(n) = O(1)
            return -1;                                                       // T(n) = O(1), S(n) = O(1)
        }

        return n - maxLength;                                                // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
