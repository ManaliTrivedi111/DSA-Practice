# 2958. Length of Longest Subarray With at Most K Frequency

# Problem Summary:

You are given an integer array nums and an integer k. A subarray is called good if every element appears at most k times within that subarray. Return the length of the longest good subarray. A subarray is a contiguous, non-empty sequence of elements from the array.

# Approach Used:

The solution uses the sliding window technique together with a HashMap to keep track of the frequency of each number inside the current window.

We maintain two pointers:
* l = left boundary of the current window.
* r = right boundary of the current window.

For every element at index r, we add it to the current window and increase its frequency in the count map. 
If the frequency of nums[r] becomes k + 1, the current window is no longer good. 
We then move the left pointer l forward and decrease the frequency of each element that leaves the window. We continue shrinking the window until the frequency of nums[r] becomes at most k. 
At this point, the window [l, r] is good, so we update ans with its length: r - l + 1

The important property of the sliding window is that both l and r only move forward. Therefore, each element is added to and removed from the window at most once.

# Steps:

1. Initialize l and r as the left and right boundaries of the sliding window.
2. Create a HashMap called count to store the frequency of each element in the current window.
3. Move r from left to right through the array.
4. Add nums[r] to the window and increase its frequency.
5. If the frequency of nums[r] becomes k + 1, move l forward.
6. Decrease the frequency of every element removed from the left side.
7. Continue until the current window becomes good again.
8. Calculate the current window length using r - l + 1.
9. Update ans with the maximum window length found.
10. Return ans.

# Solution:

```
class Solution {
    public int maxSubarrayLength(int[] nums, int k) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        int ans = 0;                                                         // T(n) = O(1), S(n) = O(1)
        Map<Integer, Integer> count = new HashMap<>();                       // T(n) = O(1), S(n) = O(1)

        for(int l = 0, r = 0; r < n; r++) {
            count.merge(nums[r], 1, Integer::sum);                           // T(n) = O(1), S(n) = O(n)

            while(count.get(nums[r]) == k + 1) {
                count.merge(nums[l++], -1, Integer::sum);                    // T(n) = O(1), S(n) = O(n)
            }

            ans = Math.max(ans, r - l + 1);                                  // T(n) = O(1), S(n) = O(1)
        }

        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
