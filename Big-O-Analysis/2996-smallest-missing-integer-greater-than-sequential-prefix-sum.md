# 2996. Smallest Missing Integer Greater Than Sequential Prefix Sum

# Problem Summary:

You are given a 0-indexed integer array nums. A prefix nums[0..i] is sequential if every element after the first is exactly 1 greater than the previous element. The prefix containing only nums[0] is always sequential.

We need to find the longest sequential prefix, calculate its sum, and then return the smallest integer that:
* Is greater than or equal to this sum.
* Does not appear anywhere in nums.

# Approach Used:

The solution uses a boolean array to record which values are present in nums.

First, we create the present array and traverse the entire nums array. For every number, we mark its corresponding position as true.
Next, we find the longest sequential prefix.

We start with the first element: sum = nums[0]
Then, starting from index 1, we continue while: nums[i] - nums[i - 1] == 1

Whenever the condition is true, the current number belongs to the sequential prefix, so we add it to sum. 
Once the sequential prefix ends, sum contains the sum of the longest sequential prefix.

Finally, we check whether sum is already present in the array.
If present[sum] is true, we increment sum and continue checking.
The first value for which present[sum] is false is the smallest missing integer greater than or equal to the sequential prefix sum.

The boolean array has a fixed size of 51 because the values involved are bounded by the problem constraints.

# Steps:

1. Create a boolean array present of size 51.
2. Traverse nums and mark every number as present.
3. Initialize sum with nums[0].
4. Start from index 1 and check whether consecutive elements differ by exactly 1.
5. If they are sequential, add the current element to sum.
6. Stop when the sequential prefix ends.
7. Starting from sum, check whether each value is present in nums.
8. If the value is present, increment sum.
9. Return the first value that is not present.

# Solution:

```
class Solution {
    public int missingInteger(int[] nums) {
        boolean[] present = new boolean[51];                                 // T(n) = O(1), S(n) = O(1)
        int len = 1;                                                         // T(n) = O(1), S(n) = O(1)
        int sum = nums[0];                                                   // T(n) = O(1), S(n) = O(1)
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        int i = 1;                                                           // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            present[num] = true;                                             // T(n) = O(1), S(n) = O(1)
        }

        while(i < n && (nums[i] - nums[i - 1] == 1)) {
            sum += nums[i];                                                  // T(n) = O(1), S(n) = O(1)
            i++;                                                             // T(n) = O(1), S(n) = O(1)
        }

        while(sum <= 50 && present[sum]) {
            sum++;                                                           // T(n) = O(1), S(n) = O(1)
        }

        return sum;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
