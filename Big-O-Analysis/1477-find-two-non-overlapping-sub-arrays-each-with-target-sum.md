# 1477. Find Two Non-overlapping Sub-arrays Each With Target Sum

# Problem Summary

Given an array arr and an integer target, find two non-overlapping sub-arrays whose sums are both equal to target.

Among all valid pairs, return the minimum possible sum of their lengths.

Return -1 if no such pair exists.

# Approach Used

The solution uses:
1. A sliding window to find sub-arrays whose sum is equal to target.
2. A minLen array where minLen[i] stores the minimum length of a valid sub-array with sum target found in the prefix arr[0..i].
3. For every right endpoint r:
   * Expand the sliding window by adding arr[r].
   * If the sum becomes greater than target, move the left pointer forward until the sum is at most target.
   * When the current window has sum target, calculate its length.
   * If there is a valid sub-array ending before the current window starts, combine its minimum length with the current length.
   * Update best, which stores the shortest valid sub-array found so far.
   * Store best in minLen[r].

Because the array elements are positive, the sliding-window approach works: when the sum exceeds target, moving l forward can only decrease the sum.

# Steps

1. Store the length of the array in n.
2. Create minLen, where minLen[i] stores the shortest valid sub-array found in arr[0..i].
3. Initialize the left pointer l, current window sum sum, answer ans, and shortest valid length best.
4. Iterate r from 0 to n - 1.
5. Add arr[r] to the current window sum.
6. While the sum is greater than target, remove elements from the left.
7. If the current window sum equals target:
   * Calculate its length.
   * If a valid sub-array exists completely before index l, combine its length with the current length.
   * Update the minimum answer.
   * Update best with the current sub-array length.
8. Store best in minLen[r].
9. Return ans if two valid non-overlapping sub-arrays were found; otherwise return -1.

# Solution

```
class Solution {
    public int minSumOfLengths(int[] arr, int target) {
        final int n = arr.length;                                            // T(n) = O(1), S(n) = O(1)
        int[] minLen = new int[n];                                           // T(n) = O(n), S(n) = O(n)
        int l = 0;                                                           // T(n) = O(1), S(n) = O(1)
        int sum = 0;                                                         // T(n) = O(1), S(n) = O(1)
        int ans = Integer.MAX_VALUE;                                         // T(n) = O(1), S(n) = O(1)
        int best = Integer.MAX_VALUE;                                        // T(n) = O(1), S(n) = O(1)

        for(int r = 0; r < n; r++) {                                         // T(n) = O(n)
            sum += arr[r];                                                   // T(n) = O(1)

            while(sum > target) {                                            // T(n) = O(n) total
                sum -= arr[l++];                                             // T(n) = O(1)
            }

            if(sum == target) {                                              // T(n) = O(1)
                int currLen = r - l + 1;                                     // T(n) = O(1)

                if(l > 0 && minLen[l - 1] != Integer.MAX_VALUE) {            // T(n) = O(1)
                    ans = Math.min(ans, currLen + minLen[l - 1]);            // T(n) = O(1)
                }
                best = Math.min(best, currLen);                              // T(n) = O(1)
            }
            minLen[r] = best;                                                // T(n) = O(1)
        }
        return ans == Integer.MAX_VALUE ? -1 : ans;                          // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n) 

# Space Complexity:
S(n) = O(n)
