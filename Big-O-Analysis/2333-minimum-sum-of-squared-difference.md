# 2333. Minimum Sum of Squared Difference

# Problem Summary

Given two integer arrays nums1 and nums2 of length n, we can modify their elements by +1 or -1 at most k1 + k2 times in total.

The goal is to minimize the sum of squared differences: sum((nums1[i] - nums2[i])²)

# Approach Used

Only the absolute difference between corresponding elements matters.

For each index, the solution computes: diff[i] = |nums1[i] - nums2[i]|

Every operation can reduce one of these differences by 1. To minimize the sum of squares, the largest differences should be reduced first because reducing a larger difference gives a greater improvement.

The solution therefore uses a frequency array cnt, where cnt[i] stores how many differences currently have value i.

It then processes differences from maxDiff down to 1:
- If there are enough operations to reduce all cnt[i] differences by 1, move all of them from i to i - 1.
- Otherwise, reduce only k of them by 1 and stop.

Finally, calculate the sum of squares from the remaining frequency counts.

# Steps

1. Compute the absolute difference for every corresponding pair.
2. Track the maximum difference and the total difference.
3. If the total difference is at most k1 + k2, all differences can be reduced to zero, so return 0.
4. Build a frequency array where cnt[d] represents the number of differences equal to d.
5. Start from maxDiff and process differences in descending order.
6. Move as many differences as possible from i to i - 1 using the available operations.
7. Stop when all operations are used.
8. Calculate the final sum of squared differences using the frequency array.

# Solution

```
class Solution {
   public long minSumSquareDiff(int[] nums1, int[] nums2, int k1, int k2) {
       final int n = nums1.length;                                           // T(n) = O(1), S(n) = O(1)
       long k = (long)(k1 + k2);                                             // T(n) = O(1), S(n) = O(1)
       int diff[] = new int[n];                                              // T(n) = O(n), S(n) = O(n)
       int maxDiff = 0;                                                      // T(n) = O(1), S(n) = O(1)
       long total = 0;                                                       // T(n) = O(1), S(n) = O(1)

       for(int i = 0; i < n; i++) {                                          // T(n) = O(n)
           diff[i] = Math.abs(nums1[i] - nums2[i]);                          // T(n) = O(1)
           maxDiff = Math.max(maxDiff, diff[i]);                             // T(n) = O(1)
           total += diff[i];                                                 // T(n) = O(1)
       }

       if(total <= k) {                                                      // T(n) = O(1)
           return 0;                                                         // T(n) = O(1)
       }

       long cnt[] = new long[maxDiff + 1];                                   // T(n) = O(maxDiff), S(n) = O(maxDiff)

       for(int i = 0; i < n; i++) {                                          // T(n) = O(n)
           cnt[diff[i]]++;                                                   // T(n) = O(1)
       }

       for(int i = maxDiff; i > 0 && k > 0; i--) {                           // T(n) = O(maxDiff)
           if(cnt[i] == 0) {                                                 // T(n) = O(1)
               continue;                                                     // T(n) = O(1)
           }

           if(k >= cnt[i]) {                                                 // T(n) = O(1)
               k -= cnt[i];                                                  // T(n) = O(1)
               cnt[i - 1] += cnt[i];                                         // T(n) = O(1)
               cnt[i] = 0;                                                   // T(n) = O(1)
           }else {
               cnt[i] -= k;                                                  // T(n) = O(1)
               cnt[i - 1] += k;                                              // T(n) = O(1)
               k = 0;                                                        // T(n) = O(1)
           }
       }

       long ans = 0;                                                         // T(n) = O(1), S(n) = O(1)

       for(int i = 0; i <= maxDiff; i++) {                                   // T(n) = O(maxDiff)
           ans += cnt[i] * ((long)i) * i;                                    // T(n) = O(1)
       }

       return ans;                                                           // T(n) = O(1)
   }
}


# Time Complexity
T(n) = O(n + D), where n = length of the arrays, and D = maximum absolute difference between corresponding elements

# Space Complexity
S(n) = O(n + D), where n = length of the arrays, and D = maximum absolute difference between corresponding elements

