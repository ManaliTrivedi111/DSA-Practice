# 3689. Maximum Total Subarray Value I

# Problem Summary:
You are given an integer array nums and an integer k. You must choose exactly k non-empty subarrays.
-> Subarrays may overlap.
-> The same subarray may be chosen multiple times.
The value of a subarray nums[l..r] is defined as: max(nums[l..r]) - min(nums[l..r])
The total value is the sum of the values of the k chosen subarrays. Return the maximum possible total value.

# Approach Used:
Since the same subarray can be chosen multiple times, we only need to find the single subarray with the maximum possible value.
The value of any subarray is: maximum element in the subarray − minimum element in the subarray
The largest possible value occurs when the subarray contains:
-> The smallest element in the entire array
-> The largest element in the entire array
The entire array always satisfies this condition because it contains every element.
Therefore, the maximum possible subarray value is: (global maximum element) − (global minimum element)
Since the same subarray may be selected repeatedly, the optimal strategy is to choose this maximum-value subarray exactly k times.
The answer becomes: (maximum value − minimum value) × k

# Steps:
1) Traverse the array and find the minimum element
2) Traverse the array and find the maximum element
3) Compute the maximum possible subarray value:
   -> maximum element − minimum element
4) Multiply the result by k
5) Return the answer

# Solution:

```
class Solution {

    public long maxTotalValue(int[] nums, int k) {

        long min = Integer.MAX_VALUE;              // T(n) = O(1), S(n) = O(1)
        long max = Integer.MIN_VALUE;              // T(n) = O(1), S(n) = O(1)

        // Loop runs n times
        for(int num : nums) {

            min = Math.min(num, min);              // T(n) = O(1) per execution
            max = Math.max(num, max);              // T(n) = O(1) per execution
        }

        return (max - min) * k;                    // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
