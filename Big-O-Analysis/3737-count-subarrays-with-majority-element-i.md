# 3737. Count Subarrays With Majority Element I

# Problem Summary:
You are given an integer array nums and an integer target. Return the number of subarrays in which target is the majority element. An element is considered the majority element of a subarray if it appears strictly more than half of the total number of elements in that subarray.

# Approach Used:
The solution uses a brute-force approach by considering every possible subarray. For each starting index, it extends the subarray one element at a time while maintaining a running score:
* Add 1 if the current element equals target.
* Subtract 1 otherwise.

For any subarray:
* Let t be the number of occurrences of target.
* Let o be the number of all other elements.
* count = t - o

Since the subarray length is t + o, the condition for target to be the majority element is: t > (t + o) / 2 
which simplifies to: t > o
or equivalently: t - o > 0

Therefore, whenever the running count becomes positive, the current subarray has target as its majority element.

# Steps:
1. Initialize the answer to 0.
2. For every starting index:
   * Initialize count to 0.
3. Extend the subarray to every possible ending index:
   * If the current element equals target, increment count.
   * Otherwise, decrement count.
   * If count > 0, increment the answer.
4. Return the total number of valid subarrays.

# Solution:

```
class Solution {
    public int countMajoritySubarrays(int[] nums, int target) {
        int n = nums.length;                              // T(n) = O(1)
        int total = 0;                                    // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {
            int count = 0;                                // T(n) = O(1)

            for(int j = i; j < n; j++) {
                if(nums[j] == target) {
                    count++;                              // T(n) = O(1) per iteration
                } else {
                    count--;                              // T(n) = O(1) per iteration
                }

                if(count > 0) {
                    total++;                              // T(n) = O(1) per iteration
                }
            }
        }

        return total;                                     // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2)

# Space Complexity:
S(n) = O(1)
