# 3471. Find the Largest Almost Missing Integer

# Problem Summary:

You are given an integer array nums and an integer k. An integer x is called almost missing if it appears in exactly one subarray of size k within nums. The goal is to return the largest almost missing integer. If no such integer exists, return -1.

# Approach Used:

The solution handles the problem using three cases based on the relationship between n and k.

1. If n == k, there is only one subarray of size k, so every value that appears in the array appears in exactly one subarray. We simply return the maximum value.

2. If k == 1, every subarray contains exactly one element. Therefore, an integer is almost missing exactly when it occurs only once in the entire array. We count the occurrences of every value and return the largest value whose count is 1.

3. Otherwise, when 1 < k < n, an element can appear in exactly one size-k subarray only if it occurs at one of the two ends of the array and does not occur elsewhere. Therefore, we check nums[0] and nums[n - 1] and return the largest valid one.

Since the values in nums are limited to the range 1 to 50, a fixed-size counting array of size 51 is sufficient.

# Steps:

1. Store the length of the array in n.
2. If n == k, scan the entire array and return its maximum value.
3. Create a counting array cnt of size 51 and count how many times each value appears.
4. If k == 1, scan the possible values from 1 to 50 and find the largest value whose total frequency is exactly 1.
5. Otherwise, check the first and last elements:
   * If both occur exactly once, return the larger one.
   * If both occur more than once, return -1.
   * If only the first occurs exactly once, return the first element.
   * Otherwise, return the last element.

# Solution:

```
class Solution {
    public int largestInteger(int[] nums, int k) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        
        if(n == k) {
            int maxNum = 0;                                                  // T(n) = O(1), S(n) = O(1)

            for(int num : nums) {
                maxNum = Math.max(maxNum, num);                              // T(n) = O(1) per iteration
            }
            return maxNum;
        }

        int[] cnt = new int[51];                                             // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            cnt[num]++;                                                      // T(n) = O(1) per iteration
        }

        if(k == 1) {
            int maxNum = -1;                                                 // T(n) = O(1), S(n) = O(1)

            for(int i = 1; i <= 50; i++) {
                if(cnt[i] == 1) {
                    maxNum = Math.max(maxNum, i);                            // T(n) = O(1) per iteration
                }
            }
            return maxNum;                                                   // T(n) = O(1)
        }else{
            if(cnt[nums[0]] == 1 && cnt[nums[n - 1]] == 1) {
                return Math.max(nums[0], nums[n - 1]);                       // T(n) = O(1)
            }else if(cnt[nums[0]] > 1 && cnt[nums[n - 1]] > 1) {
                return -1;                                                   // T(n) = O(1)
            }else if(cnt[nums[0]] == 1) {
                return nums[0];                                              // T(n) = O(1)
            }else{
                return nums[n - 1];                                          // T(n) = O(1)
            }
        }
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
