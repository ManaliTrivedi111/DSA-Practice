# 2091. Removing Minimum and Maximum From Array

# Problem Summary:

You are given a 0-indexed array of distinct integers nums. The goal is to remove both the minimum and maximum elements from the array.

A deletion can remove one element from either the front or the back of the array.

The task is to return the minimum number of deletions required to remove both the minimum and maximum elements.

# Approach Used:

The solution first finds the positions of the minimum and maximum elements in the array.

Once their positions are known, there are four possible ways to remove both elements:
* Remove both elements from the front.
* Remove both elements from the back.
* Remove the minimum from the front and the maximum from the back.
* Remove the maximum from the front and the minimum from the back.

The solution calculates the deletion count for each possibility and returns the minimum among them.

# Steps:

1. Handle the special case where the array contains only one element.
2. Traverse the array to find the minimum and maximum values and their indices.
3. Calculate the deletion count for each of the four possible strategies.
4. Return the minimum deletion count.

# Solution:

```
class Solution {
    public int minimumDeletions(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)

        if(n == 1) {
            return 1;                                                        // T(n) = O(1), S(n) = O(1)
        }

        int minNum = Integer.MAX_VALUE;                                      // T(n) = O(1), S(n) = O(1)
        int maxNum = Integer.MIN_VALUE;                                      // T(n) = O(1), S(n) = O(1)
        int minPos = -1;                                                     // T(n) = O(1), S(n) = O(1)
        int maxPos = -1;                                                     // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            if(nums[i] < minNum) {
                minNum = nums[i];                                            // T(n) = O(1)
                minPos = i;                                                  // T(n) = O(1)
            }
            if(nums[i] > maxNum) {
                maxNum = nums[i];                                            // T(n) = O(1)
                maxPos = i;                                                  // T(n) = O(1)
            }
        }

        return Math.min(minPos + 1 + n - maxPos, 
        	Math.min(maxPos + 1 + n - minPos, 
        	Math.min(Math.max(minPos + 1, maxPos + 1), 
        	Math.max(n - minPos, n - maxPos))));                               // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
